# HTTP vs HTTPS

-   How does HTTPS work? What are TLS/SSL certificates?
-   Why is this important to implement in your projects?
-   Demo how to generate certificates and use them in a node project


## What is HTTPS - A simplfied view 
*"HTTPS is simply your standard HTTP protocol slathered with a generous layer of delicious SSL/TLS encryption goodness. Unless something goes horribly wrong (and it can), it prevents people like the [infamous Eve](https://en.wikipedia.org/wiki/Alice_and_Bob#Cast_of_characters) from viewing or modifying the requests that make up your browsing experience; it’s what keeps your passwords, communications and credit card details safe on the wire between your computer and the servers you want to send this data to"* - [Robert Heaton](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)

**The MDN definition:**
HTTPS is an encrypted version of the HTTP protocol. It usually uses SSL or TLS to encrypt all communication between a client and a server. This secure connection allows clients to safely exchange sensitive data with a server, e.g. for banking activities

Servers and clients still speak exactly the same HTTP to each other, but over a secure SSL connection that encrypts and decrypts their requests and responses.

## What are SSL and TLS?


SSL - Secure Sockets Layer, standard technology for keeping an internet connection secure
It uses encryption algorithms to scramble data in transit, preventing hackers from reading it as it is sent over the connection.

TLS - (Transport Layer Security) is just an updated, more secure, version of SSL.

CERTIFICATE - Certificates are _public keys_ that correspond to a private key, and that are digitally signed either by a Certificate Authority or by the owner of the private key (such certificates are referred to as “self-signed”).

The first step to obtaining a certificate is to create a _Certificate Signing Request_ (CSR) file, which one needs to share with CA and get it signed.



---
#### A Brief History of SSL and TLS and which one you should use

SSL and TLS are both cryptographic protocols that provide authentication and data encryption between servers, machines and applications operating over a network (e.g. a client connecting to a web server). SSL is the predecessor to TLS. Over the years, new versions of the protocols have been released to address vulnerabilities and support stronger, more secure cipher suites and algorithms. TLS is currently at v. 1.3.
SSL is deprecated. You should disable SSL 2.0 and 3.0 in your server configuration, leaving only TLS protocols enabled.

SSL/TLS are used with **Certificates -- are not dependent on protocols**. That is, you don’t need to use a TLS Certificate vs. an SSL Certificate. While many vendors tend to use the phrase “SSL/TLS Certificate”, it may be more accurate to call them “Certificates for use with SSL and TLS


HTTPS (Hyper Text Transfer Protocol Secure) appears in the URL when a website is secured by an SSL certificate. The details of the certificate, including the issuing authority and the corporate name of the website owner, can be viewed by clicking on the lock symbol on the browser bar.


## How does HTTPS work? 

An SSL connection between a client and server is set up by a 'handshake*':
\*our TLS handshake occurs long before any HTTP traffic.
![](https://i.imgur.com/xkGqTZe.png)


* **Hello** 
    * The client sends a hello message
    * The message contains everything the server needs to connect to the client using SSL/TLS (i.e. which encryption algorithm they will use to exchange data, maximum SSL version)
    * The server responds with a hello
    * The server's hello contains the info that client needs (i.e. a decision based on the client's preferences)

* **Certificate Exchange** 
    * The server has to prove its identity to the client 
    * It does this by using its SSL certificate (a bit like a passport)
    * The client checks to see if it implicitly tusts the certificate or if it is verified and trusted by a certificate authority that it trusts
    * The server can also require a cerficiate from the client which proves the clients identity (only used in very senstive applications)

* **Key Exchange**
    * A symmetric algorithm is used to encrypt the message data between client and server
    * Symmetric algorithms use **one** key for encryption and decryption - both parties have to agree on the key (this process is done securely using asymmetric encryption and the server's public/private keys)
     * the client generates the key using the algorithm from hello and the server's public key found on its certificate
     * client sends the encrypted key to the server
     * server decrypts key using the its private key

When all this is done, the client and server have completed the handshake and have verified one anothers identities. 

Now Man In The Middle Attackers are unable to read or modify any requests that they may intercept.




## Certificates
* At its most basic level, an SSL certificate is simply a text file, and anyone with a text editor can create one.
* So you can't trust them all!

* There are 2 reasons why you might trust a cerficate: 
    * its on a list of certifcates that you trust implicity (browsers come with a list of trusted SSL certiticates from Certificate Authorties )
    * it’s able to prove that it is trusted by the controller of one of the certificates on the above list


**Digitial signatures**: *The certificate in encrypted form*
* **Signed certificates:**
    * An authority 'signs' (verifies) the certificate 
    * The authority uses their private key to encrypt the certificate  and this cipher (encrypted) text is attached to the certificate - its digital signature! 
    * Anyone can decrypt this using the authorities public key and verify that it results in the expected decrypted value 
    * BUT as only the authority can encrypt content (using its private key), only authorities can create valid signatures
    * This means that your browser has a way to verify that a certificate was signed by who its claiming to be signed by 

* **self-signed certificates:** 
    * all root certificates are self-signed
    * self-signed means that the digital signature is created using the certificate's own private key 
    * You can generate these yourself (see our demo)
    * BUT as the self-signed certifcate isn't pre-loaded into browsers, none of them will trust you!




## Demo - How to generate certificates and use them in a node project

You can generate a certificate on your local host in different ways: 

1. Use an **npm module** like **pem** to create keys and certificates (but no CSR) https://www.npmjs.com/package/pem
2. Use the **openssl command-line program**, which comes with most Linux and Mac OS X systems, to generate private/public keys and a CSR (Certificate Signing Request) needed to register your certificate with a CA (Certificate Authority) // See Google Web Fundamentals link
  
  In this demo we are using pem, and adding our certificate to our local machine and browswer whitelist (rather thatn the browser checking if the certificate with valid with a CA. )



```javascript=
const router = require('./router');
const https = require('https');
const pem = require('pem');

require('env2')('./config.env');

pem.createCertificate({ days: 1, selfSigned: true }, (err, keys) => {
  if (err) {
    throw err;
  }
  const options = {
    key: keys.serviceKey,
    cert: keys.certificate,
  };
  
    // 443 is the standard HTTPS port, but requires root permissions on most systems
    // you can use a higher number port like 4300 to workaround this
  https.createServer(options, router).listen(443, () => {
    console.log('Server listening on port: 443');
  });
  
});

```

### Our self-signed root certificate:
![](https://i.imgur.com/U1E6gH5.png)

### A real certificate validated by Let's Encrypt
![](https://i.imgur.com/bRZC0sF.png)

### What can we do to use our self-signed certificate on our local machines and avoid this safety warning? 
![](https://i.imgur.com/sPuVnem.png)




Easy fix for Chrome - **For development purposes only. NEVER USE THIS FOR PRODUCTION!)**
-------------------

In Chrome all you need to do is to open enter this URL [chrome://flags/#allow-insecure-localhost](chrome://flags/#allow-insecure-localhost) into the address bar to find the configuration setting for allowing invalid certificates for localhost:

[![Chrome settings](https://improveandrepeat.com/wp-content/uploads/2016/09/SelfSigned_Chrome_Settings.png)](https://improveandrepeat.com/wp-content/uploads/2016/09/SelfSigned_Chrome_Settings.png)

Click on `Enable` and restart Chrome. From now on invalid certificates on localhost (and just on localhost) are ignored and you can develop with your self-signed certificate.



## Resources 
* [How does https actually work?](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)
* [SSL vs. TLS - What's the Difference?](https://www.globalsign.com/en/blog/ssl-vs-tls-difference/)
* [An Introduction to Certification Authority Authorization (CAA)](https://www.ssl.com/article/certification-authority-authorization-caa/)
* [How to Use SSL/TLS with Node.js](https://www.sitepoint.com/how-to-use-ssltls-with-node-js/)
* [Automatically enable HTTPS on your website with EFF's Certbot, deploying Let's Encrypt certificates](https://certbot.eff.org/)
* [Lets Encrypt](https://letsencrypt.org/getting-started/)
* [Heroku SSL](https://devcenter.heroku.com/articles/ssl)
* [Google Web Fundamentals - Enabling HTTPS on Your Servers](https://developers.google.com/web/fundamentals/security/encrypt-in-transit/enable-https)
* [npm pem](https://www.npmjs.com/package/pem)
* [Stack Overflow - how to create a https server in node](https://stackoverflow.com/questions/5998694/how-to-create-an-https-server-in-node-js)
* [Medium -ssl certifcated explained](https://medium.com/nodejs-tips/ssl-certificate-explained-fc86f8aa43d4)
* [The First Few Milliseconds of an HTTPS Connection](http://www.moserware.com/2009/06/first-few-milliseconds-of-https.html)
* [Allowing Self-Signed Certificates on Localhost with Chrome and Firefox](https://improveandrepeat.com/2016/09/allowing-self-signed-certificates-on-localhost-with-chrome-and-firefox/)
