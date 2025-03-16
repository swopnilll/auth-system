## **🔹 Secure the JWT with HTTP-Only Cookies**

#### **1️⃣ Modify Login API to Send JWT as a Cookie**

```c#
[HttpPost("login")]
public async Task<IActionResult> Login([FromBody] LoginRequest request)
{
var user = await \_context.Users.SingleOrDefaultAsync(u => u.Email == request.Email);
if (user == null || !BCrypt.Net.BCrypt.Verify(request.Password, user.PasswordHash))
{
return Unauthorized("Invalid credentials");
}

    var token = GenerateJwtToken(user);

    Response.Cookies.Append("AuthToken", token, new CookieOptions
    {
        HttpOnly = true, // Prevents access from JavaScript
        Secure = true,   // Only sent over HTTPS
        SameSite = SameSiteMode.Strict
    });

    return Ok("Logged in successfully!");

}

```

## **🔹 Authorization (Restricting Access Based on Roles)**

💡 **Use JWT Claims to Restrict API Access.**

#### **Protect an API Route**

```c#
[Authorize(Roles = "Customer")]
[HttpGet("dashboard")]
public IActionResult GetDashboard()
{
    return Ok("Welcome to your dashboard!");
}

```

## **🔹 Architecture**

✅ **React Frontend** (Sends login requests, stores JWT in cookies, and includes it in every request).\
✅ **.NET 8 API** (Validates JWT tokens before granting access).\

---

## **🔹 Summary**

✅ **Step 1**: Basic Authentication (username + password).\
✅ **Step 2**: Introduced JWT Tokens.\
✅ **Step 3**: Secured JWT using **HTTP-Only Cookies**.\
✅ **Step 4**: Added Authorization using **JWT Claims**.

### **🔹 Problem with JWT-Based Authentication**

Our authentication system is **working fine**, but it has some **limitations**:

1️⃣ **Scalability Issues** → The authentication server must handle user credentials directly, making it a **single point of failure.**\
2️⃣ **No Third-Party Authentication** → Users can't log in using Google, Facebook, etc.\
3️⃣ **Re-authentication on Multiple Services** → If you have **multiple services**, users must log in separately for each.\
4️⃣ **No Standardized Identity Management** → Our system is **custom-made**, and different apps handle user authentication **differently**, leading to security risks.
