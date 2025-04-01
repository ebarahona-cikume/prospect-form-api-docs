## 🏗 **3. `domain.md` (Domain Layer)**
```md
# Domain Layer

This layer contains the **entities** and **business rules**.

## 📌 Key Components
- **Entities**: Represent the core domain.
- **Value Objects**: Immutable objects with specific rules.

## 🔧 Example of an Entity
```csharp
[Table("form_design", Schema = "frm")]
public class FormDesignModel : BaseModel
{
    [Column("ID")]
    public Guid Id { get; set; }

    [Column("name")]
    public required string Name { get; set; }

    [Column("description")]
    public string? Description { get; set; }
}
