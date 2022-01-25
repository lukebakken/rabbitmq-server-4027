# Test environment

* Windows 10 Pro
* RabbitMQ 3.9.13
* Erlang 24.2.1
* Plugins (truncated):
    ```
    C:\Users\bakkenl\rmq-server\rabbitmq_server-3.9.13> .\sbin\rabbitmq-plugins.bat list
    Listing plugins with pattern ".*" ...
     Configured: E = explicitly enabled; e = implicitly enabled
     | Status: * = running on rabbit@bakkenl-z01
     |/
    [E*] rabbitmq_management               3.9.13
    [e*] rabbitmq_management_agent         3.9.13
    [e*] rabbitmq_mqtt                     3.9.13
    [e*] rabbitmq_web_dispatch             3.9.13
    [E*] rabbitmq_web_mqtt                 3.9.13
    ```

Test the local Web MQTT TLS port *and* present a client certificate:

```
openssl s_client -connect localhost:15676 -cert ./certs/client_certificate.pem -key ./certs/client_key.pem -CAfile ./certs/ca_certificate.pem -verify 8 -verify_hostname bakkenl-z01
```

Test the local Web MQTT TLS port *without presenting* a client certificate:

```
openssl s_client -connect localhost:15676 -CAfile ./certs/ca_certificate.pem -verify 8 -verify_hostname bakkenl-z01
```

Successful output:

```
verify depth is 8
CONNECTED(0000019C)
Can't use SSL_get_servername
depth=1 CN = TLSGenSelfSignedtRootCA, L = $$$$
verify return:1
depth=0 CN = bakkenl-z01, O = server
verify return:1
---
Certificate chain
 0 s:CN = bakkenl-z01, O = server
   i:CN = TLSGenSelfSignedtRootCA, L = $$$$
 1 s:CN = TLSGenSelfSignedtRootCA, L = $$$$
   i:CN = TLSGenSelfSignedtRootCA, L = $$$$
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIDdDCCAlygAwIBAgIBATANBgkqhkiG9w0BAQsFADAxMSAwHgYDVQQDDBdUTFNH
ZW5TZWxmU2lnbmVkdFJvb3RDQTENMAsGA1UEBwwEJCQkJDAeFw0yMjAxMjUyMTM3
MzNaFw0zMjAxMjMyMTM3MzNaMCcxFDASBgNVBAMMC2Jha2tlbmwtejAxMQ8wDQYD
VQQKDAZzZXJ2ZXIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDFnfLa
FOKhRjXto5uJenAKpmGdRNtebJeoesW/cx1IO4wAL/7cATuXa8Hi0Gxs2B4gfu63
rWVMcAt+c2JrH363Fb7ia2647HQYvvcT8uDZ5QtLYI5uvkjNoMLISNijX5SJAPA9
++2BLxBaFme/mvUXH10xTQUINuxMJjsSICC9uB97xqcz4d2znQpxAM/IHuwqd9uO
GctKwRBkAPNEfXFmBCqDdzSKXorzPi6OOcqoArSfX8wv1ixWrkQno77JfAdcTdtQ
BfwC6+cMAIdAQlht8Km2vtLelDx2OJhy+2fuyQ/CPIpShExE03Sh2TPn58Rd6Lv7
yYzITFi9stcXrx51AgMBAAGjgaAwgZ0wCQYDVR0TBAIwADALBgNVHQ8EBAMCBaAw
EwYDVR0lBAwwCgYIKwYBBQUHAwEwLgYDVR0RBCcwJYILYmFra2VubC16MDGCC2Jh
a2tlbmwtejAxgglsb2NhbGhvc3QwHQYDVR0OBBYEFIAS8LAT6wI2lYbDX2UXrtsW
i0fBMB8GA1UdIwQYMBaAFFG5AbjmRzDTGd03lv/fhPh9BXbuMA0GCSqGSIb3DQEB
CwUAA4IBAQAOuHr7iRjPp7AwXW/alNthzK8/YZ06/Fr2wCK4zssjPb0jhSSYVWYu
bHb2xv93xngW6ukCH1gkiI/RIcoUmECERqTaahxIylUvVTzG2znCesI+OjrWCLDg
tmPTbthXm3LoBkLO/U2y66fEgGsi7tFDLoJQ2DZkINjmNnTmj6d8gvyeDrZbiBC5
1fPU2gGj5pyDZDpJls2KdVS4ZE/3+gyYZXjECV/Ve0SHJFXGnZ8W0/RL8o15wI33
1xW22C3gQy0FuJTqE6GPjIKGnDPS2cu2f2dANaDIR8TPTMgRDCVq9F3cp8Pdm4v5
bzCcgQ1a8mpOpReDgC5ZP3ddc/4qFhmw
-----END CERTIFICATE-----
subject=CN = bakkenl-z01, O = server

issuer=CN = TLSGenSelfSignedtRootCA, L = $$$$

---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 2301 bytes and written 373 bytes
Verification: OK
Verified peername: bakkenl-z01
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---
```
