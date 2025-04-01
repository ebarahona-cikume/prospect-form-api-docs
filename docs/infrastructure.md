## 🛠 **4. `infrastructure.md` (Infrastructure Layer)**
```md
# Infrastructure Layer

This layer implements **repositories**, **external services**, and **data persistence**.

## 📌 Key Components
- **Repositories**: Implement the contracts defined in `Domain`.
- **Persistence**: Database configuration.

## 🔧 Example of a Repository
```csharp
public class UserRepository : IUserRepository
{
    private readonly AppDbContext _context;

    public UserRepository(AppDbContext context)
    {
        _context = context;
    }

    public async Task AddAsync(User user)
    {
        _context.Users.Add(user);
        await _context.SaveChangesAsync();
    }
}
