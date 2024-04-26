# Kerberos

## Kerberos Authentication <a href="#bkmrk-page-title" id="bkmrk-page-title"></a>

When a user logs into their PC, Kerberos is used to authenticate them via mutual authentication, or both the user and the server verify their identity. Kerberos is a stateless authentication protocol based on tickets instead of transmitting user passwords over the network. As part of Active Directory Domain Services (AD DS), Domain Controllers have a Kerberos Key Distribution Center (KDC) that issues tickets. When a user initiates a login request to a system, the client they are using to authenticate requests a ticket from the KDC, encrypting the request with the user's password. If the KDC can decrypt the request (AS-REQ) using their password, it will create a Ticket Granting Ticket (TGT) and transmit it to the user. The user then presents its TGT to a Domain Controller to request a Ticket Granting Service (TGS) ticket, encrypted with the associated service's NTLM password hash. Finally, the client requests access to the required service by presenting the TGS to the application or service, which decrypts it with its password hash. If the entire process completes appropriately, the user will be permitted to access the requested service or application.

#### Kerberos Authentication Process <a href="#bkmrk-kerberos-authenticat" id="bkmrk-kerberos-authenticat"></a>

1. The user logs on, and their password is converted to an NTLM hash, which is used to encrypt the TGT ticket. This decouples the user's credentials from requests to resources.
2. The KDC service on the DC checks the authentication service request (AS-REQ), verifies the user information, and creates a Ticket Granting Ticket (TGT), which is delivered to the user.
3. The user presents the TGT to the DC, requesting a Ticket Granting Service (TGS) ticket for a specific service. This is the TGS-REQ. If the TGT is successfully validated, its data is copied to create a TGS ticket.
4. The TGS is encrypted with the NTLM password hash of the service or computer account in whose context the service instance is running and is delivered to the user in the TGS\_REP.
5. The user presents the TGS to the service, and if it is valid, the user is permitted to connect to the resource (AP\_REQ).

![Kerb\_auth.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/iFp3THqpVzwYDCUb-kerb-auth.png)

#### Authentication Service Request (AS-REQ) <a href="#bkmrk-authentication-servi" id="bkmrk-authentication-servi"></a>

AS-REQ is the message sent to KDC to request for TGT. The AS-REQ contains the following information:

* **Principal Name (PName):**\
  The AS-REQ includes the principal name of the user requesting authentication. This is typically the username or the service principal name (SPN) of the client.
* **Realm (Realm):**\
  The realm identifies the Kerberos realm to which the user or service belongs. In the context of Active Directory, the realm is often the domain name in uppercase.
* **Encryption Types:**\
  The client specifies the encryption types it supports or prefers for the Ticket Granting Ticket (TGT) that will be issued. This list indicates the cryptographic algorithms that the client can use to encrypt and protect the TGT.
* **Client Nonce (CNonce):**\
  The client generates a one-time random value known as a nonce. The AS-REQ includes this nonce, and it is used to prevent replay attacks. The KDC will include this nonce in the Ticket Granting Ticket (TGT) and subsequent tickets.
* **Supported Kerberos Options:**\
  The AS-REQ includes various flags that indicate supported Kerberos options. These options can include whether the request is for initial authentication, whether pre-authentication is required, and other relevant settings.
* **Optional Pre-Authentication Data:**\
  Optionally, the AS-REQ may include pre-authentication data, such as encrypted timestamps or other information, depending on the security settings and policies configured.

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/3fDkxNqNb74qEtAq-image.png" alt=""><figcaption></figcaption></figure>

The username and current timestamp, encrypted with the user's password is being sent to the KDC for verification.

Since KDC has the user's password stored, it will try to decrypt the message it received with the user's password. If it successfully decrypted the password, that means that the password is correct and a AS-REP will be sent back to user containing the encrypted TGT.

#### Authentication Service Respond (AS-REP) <a href="#bkmrk-authentication-servi-0" id="bkmrk-authentication-servi-0"></a>

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/pWXiAvUMYweRXbUy-image.png" alt=""><figcaption></figcaption></figure>

Now that user will decrypt the message using their password. TGS-REQ and TGS-REP too works identically to AS-REQ and AS-REP.

[<mark style="color:purple;">Notice how the password are never being transmitted and only encrypted messages. Since both the user and KDC has the user password, if they are able to decrypt the message with their own copy of password, it means they are authorized.</mark>](#user-content-fn-1)[^1]

[^1]: 
