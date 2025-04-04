# Presentation Layer

The **Presentation Layer** defines the controllers and APIs.

## 📌 Key Components
- **Controllers**: Expose the endpoints.
- **Middlewares**: Handle unexpected errors.

## 🔧 Example of an API Endpoint
```csharp
[HttpPost("saveData/{uuid}")]
public async Task<IActionResult> SaveData([FromBody] SaveFormDataRequestDTO request, string uuid)
{
    if (request.Fields == null || !(request.Fields.Count > 0))
    {
        return _responseHandler.HandleError("The property 'fields' is required", HttpStatusCode.BadRequest, null, false, null);
    }

    FieldSaveFormRequestDTO? honeypot = request.Fields.FirstOrDefault(field => field.Name!.Equals("Honeypot", StringComparison.OrdinalIgnoreCase));

    if (honeypot == null)
    {
        return _responseHandler.HandleError("The field 'honeypot' is required", HttpStatusCode.BadRequest, null, false, null);
    }

    bool doesHoneypotHaveValue = _fieldsValidatorSaveFormDataRequest.DoesHoneypotHaveValue(honeypot);

    if (doesHoneypotHaveValue)
    {
        return _responseHandler.HandleError("BOT DETECTED", HttpStatusCode.BadRequest, null, false, null);
    }

    Guid tenantId = Guid.Parse(uuid);

    try
    {
        DbSecretsModel? secret = await _secretsDbService.GetSecretByIdAsync(tenantId);

        if (secret == null)
        {
            return _responseHandler.HandleError($"The provided tenantId '{tenantId}' does not exists", HttpStatusCode.NotFound, null, false, null);
        }

        if (secret.db_name == null)
        {
            return _responseHandler.HandleError($"No database exists for the provided tenantId '{tenantId}'", HttpStatusCode.NotFound, null, false, null);
        }

        string databaseName = secret.db_name;

        // 1. Retrieve the form fields in SQL to map the data (field_id)
        FormDesignModel? formDesign = await _formDesignService.GetFirstAsync(databaseName);

        if (formDesign == null)
        {
            return _responseHandler.HandleError("No forms have been configured for the provided tenant", HttpStatusCode.InternalServerError, null, false, null);
        }

        // Retrieve form fields
        List<InputFieldModel> inputsFields = await _inputFieldService.GetByFormDesignIdAsync(formDesign.Id, databaseName, includeField: true);

        // Validate that all required fields are present
        List<string> missingFields = _fieldsValidatorSaveFormDataRequest.ValidateRequiredFields(request.Fields, inputsFields);

        if (missingFields.Count > 0)
        {
            string responseMessage = "The following fields are required and are not found within the 'fields' property";

            return BadRequest(new { Title = "Bad Request", Status = HttpStatusCode.BadRequest, Message = responseMessage, Data = new { missingFields } });
        }

        // 2. For each field, validate that the "name_input" property of each JSON object corresponds to a created form field
        ProspectResultDTO prospectResult = _prospectMapper.MapRequestToProspect(request.Fields, inputsFields);

        if (prospectResult.Success && prospectResult.Prospect != null)
        {
            await _prospectService.CreateProspectAsync(prospectResult.Prospect, secret.db_name);

            return _responseHandler.HandleSuccess("Data saved successfully.");
        }
        else
        {
            string responseMessage = "The value of some fields is not valid";

            return BadRequest(new { Title = "Bad Request", Status = HttpStatusCode.BadRequest, Message = responseMessage, Data = prospectResult.Errors });
        }
    }
    catch (ArgumentException ex)
    {
        return _responseHandler.HandleError(ex.Message, HttpStatusCode.BadRequest);
    }
    catch (Exception ex)
    {
        string errorMessage = "An error occurred while saving data.";

        if (ex.InnerException != null)
        {
            errorMessage += $" Inner exception: {ex.InnerException.Message}";
        }

        return _responseHandler.HandleError(errorMessage, HttpStatusCode.InternalServerError, ex);
    }
}
