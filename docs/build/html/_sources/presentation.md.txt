# Presentation Layer

The **Presentation Layer** defines the controllers and APIs.

## 📌 Key Components
- **Controllers**: Expose the endpoints.
- **Middlewares**: Handle authentication and validations.

## 🔧 Example of an API Endpoint
```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    private readonly CreateUserUseCase _createUserUseCase;

    public UsersController(CreateUserUseCase createUserUseCase)
    {
        _createUserUseCase = createUserUseCase;
    }

    [HttpPost]
    public async Task<IActionResult> Create([FromBody] CreateUserDto user)
    {
        await _createUserUseCase.Execute(user);
        return Ok();
    }
}
