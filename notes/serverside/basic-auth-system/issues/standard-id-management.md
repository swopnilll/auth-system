## **🔹 Problem 4: No Standardized Identity Management (Security Risk)**

### **🛑 Issue:**

Every app **implements authentication differently**, leading to **security risks.**

📌 **Scenario:**

- You create **a custom JWT authentication system.**
- A second team in your company **creates another system.**
- A third team **stores passwords incorrectly (security risk).**

📌 **Why is this a problem?**\
1️⃣ Each system has **different security policies** → Some might be **insecure.**\
2️⃣ If a developer makes **a mistake**, the whole system **is vulnerable**.\
3️⃣ **Regulatory compliance** (GDPR, ISO 27001, HIPAA) becomes difficult.

📌 **Example:**\
Imagine you work at a **bank**.

- Some branches require **PIN codes** for transactions.
- Others **just ask for your name.**
- A hacker finds the **easiest** branch to attack.

📌 **How OAuth2 + OpenID Connect Helps?**

- It **standardizes authentication** → All services **follow the same rules.**
- It's **managed by a trusted provider** (Auth0, AWS Cognito, Azure AD).
- It **reduces security risks** and improves **compliance with regulations.**
