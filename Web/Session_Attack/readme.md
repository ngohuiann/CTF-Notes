<ul><li>Session is used to store variable and passing it from one webpage to another temporarily.</li>
		<li>It is a way to make data accessible in a website.</li></ul>
    
### Example usage of session:

When a user logged in, a session was created to store the user's information. On other pages that the user visit, their username can be retrieved from the session variable and reflect back to the user. 
    
## Session fixation
Session ID is commonly used in web application to authenticate user. When a session ID is leaked, the attacker can use the ID to get access to the user's account.\
Session fixation attack scenario:
		<ol><li>Attacker login to the system to get a session ID.</li>
			<li>Attacker craft a login link with the session ID and send it to the victim.</li>
			<li>Victim clicks on the link and login to the system with their credential.</li>
			<li>Attacker is able to access the system with the session ID.</li></ol>

To prevent session fixation attack:
		<ul><li>Changing session ID after user logged in.</li>
			<li>Validate the session ID before passing it to the server</li>
			<li>Do not pass session ID from HTTP requests and headers.</li>
			<li>Set a timeout value for the session.</li></ul>

## Session hijack
Session hijacking is done to gain access to a session. Unlike session fixation where a session is created and send to the victim, session hijack gain access to the session by stealing the victim's session.\
This can be done by:
<ul><li>Predicting the session ID</li>
		<li>Network traffic sniffing</li>
		<li>Cross-site scripting</li>
		<li>Man-in-the-middle (MiTM) attack</li></ul>
To prevent session hijacking:
<ul><li>SSL</li>
		<li>VPN for ecryption</li>
		<li>Generate random session ID</li>
		<li>Set a timeout value for the session</li></ul>
