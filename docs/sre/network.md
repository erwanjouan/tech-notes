### What happens when you click on a link ?

https://www.freecodecamp.org/news/what-happens-when-you-hit-url-in-your-browser/

https://tara-ojo.medium.com/what-happens-when-you-click-a-link-in-the-browser-83b4337c78dc

![a](./hyperlink.files/dns.png)

![a](./hyperlink.files/flow.png)

### Reverse proxy

E2E internet communication:

![a](./proxy.files/global.png)

Forward proxy:

![a](./proxy.files/forward.png)

Reverse proxy:

![a](./proxy.files/reverse.png)

Reverse proxy with firewall:

![a](./proxy.files/fw.gif)

BD (Octo talks)

https://blog.octo.com/bd-le-proxy-et-le-reverse-proxy/

![a](./proxy.files/bd.png)

### Quic

As Numerama is hosted by Cloudflare, they use Quic as session protocol (instead of Https).

https://cloudflare-quic.com/

https://blog.cloudflare.com/http3-the-past-present-and-future/

### HTTPS

![type:video](https://www.youtube.com/embed/T4Df5_cojAs)

#### Prerequisites

- You need to assume:
  - Any message encrypted with Bob's public key can only be decrypted with Bob's private key
  - Anyone with access to Alice's public key can verify that a message (signature) could only have been created by someone with access to Alice's private key

#### Certificate Authority

Certificate of youtube is signed by Google CA. Both youtube and Google have a keyPair.

To use HTTPS, Youtube makes "Certificate Signing Request" made from its keyPair to Google CA.

In return, Google CA signs the certificate with its private key, anyone who has the public key from Google CA can verify the CA is who he claims to be.

Most browsers have a list of certificate that are delivered by known CA.


#### Self signed certificate

To communicate securely with an app on staging env, you need to create an app with a private key and a public key. Next step would be to create a signing request, but no ones will sign this certificate.

In this case, a second pair of keys is created, by a CA you just created.

When a client connects to this app, the public key of the app gets retrieved and the client can detect it is not signed by a trusted authority.

The constaint is that you have to tell your client that the newly created CA can be trusted.

#### Public Key Certificate

![a](./https.files/public_key_certif.png)

- Resolves the problem of public key forgery.
  - Certificate is basically a set of properties in clear text, associated with a period of validity and a public key. 
  - This piece of information is sent to CA where it is suffixed by CA information. The whole is hashed and encrypted (or signed) by CA private key. The certificate is now signed and can be sent everywhere. 
  - When someone downloads the certicate, he has access to CA public key and hence can decrypt the signing part, which gives the hashed data. But the receiver can also take the clear part of the certificate and hash it on his side. If both hash match, the certificate has been properly signed. The public key is validated.

![type:video](https://www.youtube.com/embed/704dudhA7UI)


### TLS

https://medium.com/jspoint/a-brief-overview-of-the-tcp-ip-model-ssl-tls-https-protocols-and-ssl-certificates-d5a6269fe29e

#### TLS v1.3

https://www.davidwong.fr/tls13/

https://tls13.ulfheim.net/

https://tools.ietf.org/html/rfc3552

#### TLS v1.2

![type:video](https://www.youtube.com/embed/86cQJ0MMses)

![a](./tls.files/tls1.png)
![a](./tls.files/tls2.png)
![a](./tls.files/tls3.png)

#### Diffie Hellman high level overview

![type:video](https://www.youtube.com/embed/NmM9HA2MQGI)

Avoids the need to exchange keys by combining public variables and private secrets.
Alice and Bob agrees first on public variables they are going to use.
g : generator = small number
n : big prime number

```mermaid
sequenceDiagram
    participant Alice
    participant Public
    participant Bob
    Note over Alice,Bob: Alice's PK = A, Let G = bigNumber, Bob's PK = B
    Public->>Alice:Get G
    Note over Alice: Compute A*G
    Public->>Bob:Get G
    Note over Bob: Compute B*G
    Bob->>Alice: Send B*G
    Note over Alice: Compute A*B*G
    Alice->>Bob: Send A*G
    Note over Bob: Compute A*B*G
    Note over Alice,Bob: Alice share the same key A*B*G, whoch never got over the wire
```