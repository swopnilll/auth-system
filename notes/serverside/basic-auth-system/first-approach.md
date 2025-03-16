## **🔹 Step 1: Basic Authentication (Username & Password)**

Imagine you have a **React frontend**, a **.NET 8 REST API**, and a **PostgreSQL database** running on **AWS**.

💡 **Goal:**\
✅ A user should be able to **register** and **log in** using email and password.\
✅ After logging in, they should be able to access protected data.

💻 **How It Works (Basic Approach):**\
1️⃣ **User registers** → React frontend sends email & password to API.\
2️⃣ **API saves credentials** (password is **hashed** in the database).\
3️⃣ **User logs in** → API checks credentials and responds with **session information**.

---

### **🔹 Implementation (Step 1)**

#### **1️⃣ Backend (.NET 8 API)**

##### **User Model (Database)**

```c#
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public string PasswordHash { get; set; } // Hashed password
}

```

##### **Authentication Controller**

```c#
using Microsoft.AspNetCore.Mvc;
using System.Security.Cryptography;
using Microsoft.EntityFrameworkCore;

[ApiController]
[Route("api/auth")]
public class AuthController : ControllerBase
{
    private readonly AppDbContext _context;

    public AuthController(AppDbContext context)
    {
        _context = context;
    }

    // 1️⃣ Register API
    [HttpPost("register")]
    public async Task<IActionResult> Register([FromBody] RegisterRequest request)
    {
        var userExists = await _context.Users.AnyAsync(u => u.Email == request.Email);
        if (userExists) return BadRequest("User already exists!");

        var hashedPassword = BCrypt.Net.BCrypt.HashPassword(request.Password); // Securely hash password

        var user = new User
        {
            Name = request.Name,
            Email = request.Email,
            PasswordHash = hashedPassword
        };

        _context.Users.Add(user);
        await _context.SaveChangesAsync();

        return Ok("User registered successfully!");
    }

    // 2️⃣ Login API
    [HttpPost("login")]
    public async Task<IActionResult> Login([FromBody] LoginRequest request)
    {
        var user = await _context.Users.SingleOrDefaultAsync(u => u.Email == request.Email);
        if (user == null || !BCrypt.Net.BCrypt.Verify(request.Password, user.PasswordHash))
        {
            return Unauthorized("Invalid credentials");
        }

        return Ok("User logged in!");
    }
}

```

##### **Database Context**

```c#
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }
    public DbSet<User> Users { get; set; }
}

```

### **🔹 Problems with this Approach**

🚨 **Major Issues:**\
1️⃣ **No Session Management** -- Once logged in, the frontend has no way to remember the user.\
2️⃣ **Sending Passwords in Every Request** -- If every API request requires email & password, it's insecure.
