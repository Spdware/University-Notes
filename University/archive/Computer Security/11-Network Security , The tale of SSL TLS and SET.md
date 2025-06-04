# Issues of Communication Security
- Problem of remoteness
- Internet protocol problems
- Transparence and critical mass problem
Ideally you want completely transparence.
## A tale  of two protocols
Two protocols tried to resolve these issues:
- HTTP over SSL or HTTPS
- SET
Both of them make sense from a technical point
### SSL -> TLS
TLS until v1.2 had some problems, but then we have 20 years before v1.3. TLS has 
v1.3 delete all the legacy problems(ex: guarantee integrity by default, almost future proof in a technical standpoint). We are almost at 80% adoption of TLS 1.3. It also does server side authentication, client authentication is optional. It is transparent for the user. Use asymmetric and symmetric  cryptography. 
#### Cipher suite
Clients and servers may use different suites of algorithms for different functions so during the handshake cipher will select the keys used in the different hash functions.
Then the client should also verify the server certificate. Validation may be trivial but still need attention to not fall into simple tricks.
Creating a certificate and maintain it can be a good way to identify clients and can be less painful than remembering and check passwords.
If you lack the pre-master phase(so you do not have the shared key) you  cannot read anything in the communication, so if you try to cut in the communication both part can understand and cut the communication with you.
### TLS pro and cons
Pros: 
- Protect transmissions
- Ensure authentication
Cons:
- Do not protect before and after the communications
- Relies on public infrastructure
- Not foolproof(example is mail clients)
Certificate Authorities are a real problem for TLS as if one goes rogue/is hacked TLS will still trust the certificates until it is discoverd that the CA is not reliable anymore. 
### Overcoming TLS/PKI limitations
**HSTS(HTTP Strict Transport Security)**
Require the browser to always connect to a domain using HTTPS. Browsers implement a HSTS preload list some websites are simply never loaded 
**HPKP(HTTP Public Key Pinning)**
Fast validating approach where the certificate is so good that the browser will remember the CA and do not accept other CAs(obsolete)
**Certificate transparency**
When a CA sign a certificate send also the metadata to a log. Then the CA embed the receipt to the certificate to the client and so the server can verify the signature in the receipt and also ask to the log server if the certificate really exist(make accountable for the certificate). Problem if the CT logs are not reachable there is a problem, the CT logs should not accept anything that is not a clean certificate to do not permit tampering with the log.
## SET introduction
Protect transactions and not connections. 
### Dual Signature Generation
Take the order information and payment instructions and hash them separately and then hash again the digest of the two previous hash. 

### Why SET failed
SET requires a certificate from the merchant, the payment gateway and the cardholder. The cardholder certificate is impossible to scale so we need a pre-registration for the them. It is not transparent so we have a less critical mass so we fail.
Nowadays a simple redirect with a token to the website of the bank is commonly used.

