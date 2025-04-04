# Domain Layer
Contains the enterprise-wide business rules. This layer is the most stable and contains the **models** that map the **entities** that we are using throught the project.

## 📌 Key Components
- **Models**: Represent the core domain.

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
