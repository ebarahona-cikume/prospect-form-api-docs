
### 🎭 **5. `presentation.md` (Capa de Presentación)**
```md
# Capa de Presentación

La capa **Presentation** define los controladores y APIs.

## 📌 Principales Componentes
- **Controllers**: Exponen los endpoints.
- **Middlewares**: Gestionan autenticación y validaciones.

## 🔧 Ejemplo de un Endpoint
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
