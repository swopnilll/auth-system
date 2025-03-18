A malicious website (`hacker.com`) **cannot read** HTTP-only cookies (which store the JWT), but it **can send an HTTP request** that includes the cookies.

### **🔍 Why Is This a Problem?**

- **Browsers automatically attach cookies** (including HTTP-only ones) to requests sent to the target domain (`api.example.com`).
- If the victim **is logged in**, their **JWT cookie will be included** in the request **even if it's sent from a malicious website**.

### **🚨 Example Attack Scenario (Without CSRF Protection)**

1.  **User logs into `example.com`**, and the backend **sets an HTTP-only JWT cookie** (`authToken`).
2.  The user visits a **malicious website** (`hacker.com`).
3.  The malicious website runs:

    ```js
    fetch("https://api.example.com/delete-account", {
      method: "POST",
      credentials: "include",
    });
    ```

4.  The browser **automatically includes the JWT cookie** in the request.
5.  If **no CSRF protection** is in place, the request **succeeds**, and the victim's account is deleted.

### **🛡️ How Do We Prevent This?**

✅ **CSRF Tokens (Double Submit or Synchronizer Token)**

- The backend **sends a CSRF token** in a response (stored in a **regular cookie or in-memory** on the frontend).
- The frontend **must attach this token** in the `X-XSRF-TOKEN` header for every state-changing request.
- The backend **verifies the CSRF token before processing the request**.
- **A malicious site (`hacker.com`) CANNOT access this token**, so their requests fail.

✅ **SameSite=Strict or SameSite=Lax Cookies**

- Setting the cookie with `SameSite=Strict` ensures **cookies are NOT sent** when requests come from another site.
- Setting `SameSite=Lax` **prevents CSRF on most POST requests** (but allows cookies on GET).

✅ **CORS Restrictions**

- Only allow `https://frontend.com` to send requests to `https://api.example.com`.
- Block `hacker.com` using `Access-Control-Allow-Origin`.

✅ **Custom Headers**

- CSRF-protected APIs require a custom `X-Requested-With` header, which browsers **don’t send automatically**.

### **📌 Conclusion**

🚀 **Yes, hackers can send requests with HTTP-only cookies attached, but they CANNOT bypass CSRF protection if implemented correctly.**
