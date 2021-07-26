# Cross-Site Scripting (XSS)
[HOME](/index.html) / [Web](/Web)

## The basics
Injection of scripts to the web.
```JavaScript
<script>alert(1);</script>

```
## Stored XSS
Most commonly seen in forums and comment area. The injected script will be stored into the server and database. 


## Reflected XSS
Inputs that can be reflected back to the user is highly vulnerable when not properly handled. 
<!--- Reflected attacks are those where the injected script is reflected off the web server, such as in an error message, search result, or any other response that includes some or all of the input sent to the server as part of the request. Reflected attacks are delivered to victims via another route, such as in an e-mail message, or on some other website. When a user is tricked into clicking on a malicious link, submitting a specially crafted form, or even just browsing to a malicious site, the injected code travels to the vulnerable web site, which reflects the attack back to the user’s browser. The browser then executes the code because it came from a “trusted” server. Reflected XSS is also sometimes referred to as Non-Persistent or Type-II XSS. --->

## DOM-Based XSS
Focuses on the client side. DOM-based XSS is a type of attack that tries to inject script to the Document Object Model (DOM).
<!--- DOM Based XSS (or as it is called in some texts, “type-0 XSS”) is an XSS attack wherein the attack payload is executed as a result of modifying the DOM “environment” in the victim’s browser used by the original client side script, so that the client side code runs in an “unexpected” manner. That is, the page itself (the HTTP response that is) does not change, but the client side code contained in the page executes differently due to the malicious modifications that have occurred in the DOM environment. --->
