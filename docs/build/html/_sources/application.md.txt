# Application Layer

The **Application Layer** contains the use cases of the application.

## 📌 Key Components
- **Services**: Contains the business logic.
- **DTOs**: Defines the input and output data.

## 🔧 Example of a Service
```csharp
public class FormDesignService(IDbConnectionService dbConnectionService, IDbContextFactory dbContextFactory, IFormDesignRepository formDesignRepository) : IFormDesignService
{
    private readonly IDbConnectionService _dbConnectionService = dbConnectionService;
    private readonly IDbContextFactory _dbContextFactory = dbContextFactory;
    private readonly IFormDesignRepository _formDesignRepository = formDesignRepository;

    public async Task<FormDesignModel?> GetFirstAsync(string tenantDatabase)
    {
        (string server, string username, string password) = await _dbConnectionService.GetTenantConnectionParams();

        if (server.IsNullOrEmpty() || username.IsNullOrEmpty() || password.IsNullOrEmpty())
        {
            throw new Exception("Could not retrieve parameters to establish connection to database in FormDesignService");
        }

        using TenantDbContext? dbContext = _dbContextFactory.CreateTenantDbContext(server, tenantDatabase, username, password);

        return dbContext == null
            ? throw new Exception("An unexpected error ocurred while database context was generated in FormDesignService")
            : await _formDesignRepository.FindFirstAsync(dbContext);
    }
}
