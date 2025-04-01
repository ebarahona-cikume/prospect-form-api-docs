# Infrastructure Layer

This layer implements **repositories**, **external services**, and **data persistence**.

## 📌 Key Components
- **Repositories**: Implement the contracts defined in `Domain`.
- **Persistence**: Database configuration.

## 🔧 Example of a Repository
```csharp
public class FormDesignRepository() : IFormDesignRepository
{
    public async Task<FormDesignModel?> FindFirstAsync(TenantDbContext context)
    {
        try
        {
            return await context.FormDesign.FirstOrDefaultAsync();
        }
        catch (Exception ex)
        {
            throw new InvalidOperationException("Error retrieving form design: ", ex);
        }
    }
}
