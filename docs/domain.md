## 🏗 **3. `domain.md` (Domain Layer)**
```md
# Domain Layer

This layer contains the **entities** and **business rules**.

## 📌 Key Components
- **Entities**: Represent the core domain.
- **Value Objects**: Immutable objects with specific rules.

## 🔧 Example of an Entity
```csharp
public class User
{
    public Guid Id { get; private set; }
    public string Name { get; private set; }
    public string Email { get; private set; }

    public User(string name, string email)
    {
        Id = Guid.NewGuid();
        Name = name;
        Email = email;
    }
}
