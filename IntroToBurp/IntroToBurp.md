---
title: IntroToBurp
platform: picoCTF
category: Web Exploitation
difficulty: Easy
points: 100
status: Solved
date: 2026-07-11
tags:
  - picoCTF
  - BurpSuite
  - HTTP
  - WebExploitation
---

# IntroToBurp

> [!info] Challenge Information
>
> **Platform:** picoCTF 2024
>
> **Category:** Web Exploitation
>
> **Difficulty:** Easy
>
> **Points:** 100

---

# 📖 Challenge Overview

The **IntroToBurp** challenge introduces the fundamental workflow of **Burp Suite**, a widely used web application security testing tool. Instead of focusing on exploiting a complex vulnerability, the challenge is designed to teach participants how to intercept, inspect, and manipulate HTTP requests exchanged between a browser and a web application.

Through this exercise, participants gain practical experience using Burp Suite's Proxy and Repeater modules while learning how authentication workflows can be analyzed from the network layer.

---

# 🎯 Objective

> [!tip]
> The goal is **not** to discover a vulnerability through guessing or brute force.
>
> The objective is to understand how Burp Suite can intercept HTTP traffic, inspect requests, and replay modified requests to observe changes in server behaviour.

---

# 🛠 Environment

| Component | Value |
|-----------|------|
| Platform | picoCTF |
| Tool | Burp Suite Community Edition |
| Browser | Chromium |
| Category | Web Exploitation |

---

# 🗺 Walkthrough

---

## Step 1 — Launch the Challenge

> [!info]
> The first step is to launch the challenge instance from the picoCTF dashboard.

Once launched, picoCTF provisions a temporary web application and provides a dedicated URL for testing.

![[Images/01-launch-instance.png]]

---

## Step 2 — Open the Challenge URL

Opening the generated URL displays a registration page.

The application contains several fields including:

- Full Name
- Username
- Phone Number
- City
- Password

At this point no obvious vulnerability is visible from the user interface.

![[02-registration-page.png]]

---

## Step 3 — Start Burp Suite

Burp Suite Community Edition was launched.

A temporary project was selected followed by the default configuration.

This creates an intercepting proxy that sits between the browser and the web application.

![[Pasted image 20260711153731.png]]

![[Pasted image 20260711153754.png]]

![[Pasted image 20260711153812.png]]

---

## Step 4 — Register a User

A new account was created by filling all required fields.

The registration itself is not the objective of the challenge.

Instead, this action generates HTTP requests that Burp Suite can inspect.

![[Pasted image 20260711153948.png]]

---

## Step 5 — Reach the OTP Page

After registration the application redirected to an OTP verification page.

This confirms that authentication consists of two stages.

```text
Registration
      │
      ▼
OTP Verification
      │
      ▼
Dashboard
```

![[Pasted image 20260711154002.png]]

---

## Step 6 — Enable Burp Intercept

Before submitting the OTP, **Intercept** was enabled inside Burp Suite.

> [!note]
> Every outgoing request is now paused before reaching the server.

![[Pasted image 20260711154029.png]]

---

## Step 7 — Capture the Request

An example OTP value was entered and submitted.

Burp intercepted the POST request.

The request body contained:

```http
otp=1234
```

The captured request also included:

- HTTP Headers
- Cookies
- POST Body
- Session Information

![[Pasted image 20260711154132.png]]

---

## Step 8 — Send the Request to Repeater

Instead of forwarding the request immediately, it was sent to **Repeater**.

Repeater allows requests to be modified and replayed multiple times without interacting with the browser.

![[Pasted image 20260711154146.png]]

---

## Step 9 — Observe Server Behaviour

The original request was replayed without modification.

Server response:

```text
Invalid OTP
```

This confirms that the server validates the supplied OTP value.

![[Pasted image 20260711154159.png]]

---

## Step 10 — Modify the Request

The request body was edited.

Original request

```http
otp=1234
```

Modified request : Remove the opt=1234

```http

```

The modified request was then replayed.

![[Pasted image 20260711154213.png]]

---

## Step 11 — Successful Response

After replaying the modified request, the application accepted the request and completed the authentication workflow.

The server returned the challenge flag.

> [!success]
> Challenge Solved ✅

![[Pasted image 20260711154516.png]]

---

# 🔍 Technical Analysis

The challenge demonstrates how HTTP requests can be intercepted and modified before reaching the server.

Instead of interacting with the application through its graphical interface, Burp Suite allows security testers to communicate directly with the application's HTTP layer.

By manipulating request parameters and replaying requests, it becomes possible to observe how the server validates user input and whether authentication logic is correctly enforced.

This exercise highlights the importance of proper server-side validation and demonstrates why client-side input should never be trusted.

---

# 📚 What I Learned

> [!success]

- Launching Burp Suite projects
- Configuring an intercepting proxy
- Capturing HTTP requests
- Understanding POST requests
- Sending requests to Repeater
- Modifying request parameters
- Observing changes in server responses
- Basics of authentication workflow testing

---

# 🧠 Key Concepts

| Concept | Description |
|----------|-------------|
| Burp Proxy | Intercepts HTTP requests |
| Repeater | Replays modified requests |
| HTTP POST | Sends form data |
| Request Body | Contains submitted user data |
| Server-side Validation | Validates user input on the server |

---

# 🛠 Tools Used

- Burp Suite Community Edition
- Chromium Browser
- picoCTF

---

# 💭 Conclusion

The IntroToBurp challenge provides a practical introduction to Burp Suite and demonstrates how HTTP requests can be intercepted, inspected, and replayed. Although the challenge is beginner-friendly, it introduces essential concepts used throughout web application penetration testing, including request analysis, parameter manipulation, and server-side validation. These skills serve as the foundation for more advanced web security assessments.

---

---

# 👨‍💻 About the Author

Hi! I'm **Karthi R**, a Cybersecurity student passionate about Web Application Security, Linux, and Offensive Security. I use this repository to document my CTF journey, improve my technical skills, and share knowledge with the cybersecurity community.

## Connect With Me

- 🌐 **GitHub:** https://github.com/karthitom
- 💼 **LinkedIn:** https://www.linkedin.com/in/karthi-jk/
- 📧 **Email:** jkarthi5680@gmail.com

---

> ⭐ If you found this writeup helpful, consider giving this repository a star!