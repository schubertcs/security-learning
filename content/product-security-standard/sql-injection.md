---
title: "Sql Injection"
date: 2025-11-05T21:49:28+01:00
draft: true
categories: ["Foundations"]
tags: ["security"]
description: "Product Security Standard guideline."
weight: 1
---

## 1. Summary (for Managers & Product Owners)
Explain what this standard is about and why it matters from a business and risk perspective.

> Example: SQL injection vulnerabilities can lead to complete data disclosure and system compromise. This standard defines how to prevent them.

---

## 2. Security Principle
State the core rule of this standard.

> Example: Always use parameterized queries and never concatenate user input into SQL statements.

---

## 3. Developer Guidance
Detailed explanation and best practices for implementing the standard.  
Include recommendations, language-specific notes, and secure defaults.

---

## 4. Implementation Example
Provide code snippets demonstrating both **unsafe** and **secure** approaches.

```java
// bad
String query = "SELECT * FROM users WHERE name = '" + userInput + "'";

// good
PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE name = ?");
ps.setString(1, userInput);
```

## 5. Checklist

✅ Use parameterized queries
✅ Validate user inputs
❌ Never concatenate SQL strings

## 6. Related Standards

- PSS #1: Don't Roll Your Own Crypto
- PSS #3: Input Validation  