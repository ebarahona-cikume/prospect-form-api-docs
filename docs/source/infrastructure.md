# Infrastructure Layer
It contains the contexts that handle the database access and the repositories to handle database operations.

## 📌 Key Components
- **DbContexts**: Handle the connection to databases.
- **Repositories**: Implement databases operations, like creating, reading and updating objects.

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
