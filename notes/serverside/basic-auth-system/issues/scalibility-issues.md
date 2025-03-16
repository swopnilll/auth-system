## **🔹 Problem 1: Scalability Issues (Single Point of Failure)**

### **🛑 Issue:**

If your authentication system is built **inside your API**, it means that **every user login request** goes through your **backend server**.

Imagine you have a **React frontend** and a **.NET 8 REST API** that uses a **PostgreSQL database** to store users.

📌 **Scenario:**

- You host the **backend** on an **AWS EC2 instance**.
- Your app gets **10,000+ logins per minute**.
- The backend **slows down** because it's handling too many authentication requests.

📌 **Why is this a problem?**\
1️⃣ If the **backend crashes**, **nobody can log in.**\
2️⃣ More users = **more database queries** = **higher costs.**\
3️⃣ If you want to **scale up**, you must **replicate the entire API**, which is complex.

📌 **How OAuth2 + OpenID Connect Helps?**

- Instead of **handling authentication ourselves**, we **delegate it** to a **separate Identity Provider (IdP)** like **Auth0, Google, or AWS Cognito**.
- The **IdP manages login**, so our backend only deals with **API requests**, not authentication.
