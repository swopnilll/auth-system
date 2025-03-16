## **🔹 Step 2: Introducing JWT Tokens**

💡 **Solution:**\
✅ When a user logs in, we send them a **JWT token** (JSON Web Token).\
✅ The frontend stores the token and sends it in **every request** to prove authentication.

---

### **🔹 How JWT Works**

1️⃣ **User logs in** → Server verifies credentials and generates a **JWT token**.\
2️⃣ **Token is sent** to the frontend.\
3️⃣ **Frontend stores the token** (either in localStorage, sessionStorage, or HTTP-only cookies).\
4️⃣ **For every API request**, the frontend sends the token in the **Authorization header**.\
5️⃣ **API verifies the token** before granting access.

---

### **🔹 Implementation (Step 2)**

#### **1️⃣ Backend: Modify Login to Generate JWT**

```c#
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using Microsoft.IdentityModel.Tokens;
using System.Text;

// JWT Secret Key
private const string SecretKey = "THIS_IS_A_SUPER_SECRET_KEY"; // Store in environment variables

private string GenerateJwtToken(User user)
{
var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(SecretKey));
var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

    var claims = new[]
    {
        new Claim(ClaimTypes.Name, user.Name),
        new Claim(ClaimTypes.Email, user.Email),
        new Claim(ClaimTypes.Role, "Customer") // Assign role
    };

    var token = new JwtSecurityToken(
        issuer: "myapp.com",
        audience: "myapp.com",
        claims: claims,
        expires: DateTime.UtcNow.AddHours(1),
        signingCredentials: credentials
    );

    return new JwtSecurityTokenHandler().WriteToken(token);

}

[HttpPost("login")]
public async Task<IActionResult> Login([FromBody] LoginRequest request)
{
var user = await \_context.Users.SingleOrDefaultAsync(u => u.Email == request.Email);
if (user == null || !BCrypt.Net.BCrypt.Verify(request.Password, user.PasswordHash))
{
return Unauthorized("Invalid credentials");
}

    var token = GenerateJwtToken(user);
    return Ok(new { token });

}

```

---

### **🔹 Problems with JWT in LocalStorage**

🚨 **Security Issues:**\
1️⃣ **XSS Attacks** -- If an attacker injects JavaScript into your app, they can steal the JWT from localStorage.

💡 **Solution:**\
✅ **Use HTTP-Only Cookies** -- The browser stores the JWT **securely** and **prevents JavaScript access**.
