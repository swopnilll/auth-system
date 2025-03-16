## **🔹 Problem 3: Re-authentication on Multiple Services**

### **🛑 Issue:**

Your app may have **multiple services**, but users **must log in separately for each.**

📌 **Scenario:**

- You build **a React web app** and a **mobile app.**
- A user logs in **on the web app**.
- Now, they open the **mobile app** and must **log in again.**
- They open a **customer support portal** and must **log in again.**

📌 **Why is this a problem?**\
1️⃣ It's **annoying** for users to **log in multiple times**.\
2️⃣ They **forget passwords**, leading to **password reset requests.**\
3️⃣ It creates **a bad user experience (UX).**

📌 **Example:**\
Imagine you go to **a theme park**.

- At every ride, they **ask you for a new ticket** instead of just giving you a **wristband**.
- Wouldn't it be easier if you could **use one wristband for all rides**?

📌 **How OAuth2 + OpenID Connect Helps?**

- With OAuth2 + OIDC, we use **Single Sign-On (SSO)**.
- The user logs in **once**, and the session **is valid across all services.**
- This is how **Google, Microsoft, and AWS manage logins across multiple apps.**
