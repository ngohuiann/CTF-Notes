# Session Attack

[HOME](../index.html) / [Web](./)

## The basics

* Session is used to store variable and passing it from one webpage to another temporarily.
* It is a way to make data accessible in a website.

### Example usage of session:

When a user logged in, a session was created to store the user's information. On other pages that the user visit, their username can be retrieved from the session variable and reflect back to the user.

## Session fixation

Session ID is commonly used in web application to authenticate user. When a session ID is leaked, the attacker can use the ID to get access to the user's account.

Session fixation attack scenario:

1. Attacker login to the system to get a session ID.
2. Attacker craft a login link with the session ID and send it to the victim.
3. Victim clicks on the link and login to the system with their credential.
4. Attacker is able to access the system with the session ID.

To prevent session fixation attack:

* Changing session ID after user logged in.
* Validate the session ID before passing it to the server
* Do not pass session ID from HTTP requests and headers.
* Set a timeout value for the session.

## Session hijack

Session hijacking is done to gain access to a session. Unlike session fixation where a session is created and send to the victim, session hijack gain access to the session by stealing the victim's session.\
This can be done by:

* Predicting the session ID
* Network traffic sniffing
* Cross-site scripting
* Man-in-the-middle (MiTM) attack

To prevent session hijacking:

* SSL
* VPN for ecryption
* Generate random session ID
* Set a timeout value for the session
