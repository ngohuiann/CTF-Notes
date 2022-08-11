# Cross-Site Scripting (XSS)

[HOME](../index.html) / [Web](./)

## The basics

Injection of scripts to the web.

```
<script>alert(1);</script>
```

## Stored XSS

Most commonly seen in forums and comment area. The injected script will be stored into the server and database.

## Reflected XSS

Inputs that can be reflected back to the user is highly vulnerable when not properly handled.

\## DOM-Based XSS Focuses on the client side. DOM-based XSS is a type of attack that tries to inject script to the Document Object Model (DOM).
