# 👤 Access control vulnerabilities and privilege escalation
In this section, we describe:

  - Privilege escalation.
  - The types of vulnerabilities that can arise with access control.
  - How to prevent access control vulnerabilities.

## ❓ What is access control?
Access control is the application of constraints on who or what is authorized to perform actions or access resources.
In the context of web applications, access control is dependent on authentication and session management:

  - Authentication confirms that the user is who they say they are.
  - Session management identifies which subsequent HTTP requests are being made by that same user.
  - Access control determines whether the user is allowed to carry out the action that they are attempting to perform.

Broken access controls are common and often present a critical security vulnerability.
Design and management of access controls is a complex and dynamic problem that applies business,
organizational, and legal constraints to a technical implementation.
Access control design decisions have to be made by humans so the potential for errors is high.

<img width="781" height="461" alt="sql-injection" src="https://github.com/user-attachments/assets/5667841b-b69d-465e-a191-d4c0693a44a1" />

## 1️⃣ Vertical access controls
Vertical access controls are mechanisms that restrict access to sensitive functionality to specific types of users.

With vertical access controls, different types of users have access to different application functions. For example, an administrator might be able to modify or delete any user's account, while an ordinary user has no access to these actions. Vertical access controls can be more fine-grained implementations of security models designed to enforce business policies such as separation of duties and least privilege.

## 2️⃣ Horizontal access controls
Horizontal access controls are mechanisms that restrict access to resources to specific users.

With horizontal access controls, different users have access to a subset of resources of the same type. For example, a banking application will allow a user to view transactions and make payments from their own accounts, but not the accounts of any other user.

## 3️⃣ Context-dependent access controls
Context-dependent access controls restrict access to functionality and resources based upon the state of the application or the user's interaction with it.

Context-dependent access controls prevent a user performing actions in the wrong order. For example, a retail website might prevent users from modifying the contents of their shopping cart after they have made payment.

## 🧩 Broken access controls
Broken access control vulnerabilities exist when a user can access resources or perform actions that they are not supposed to be able to.

