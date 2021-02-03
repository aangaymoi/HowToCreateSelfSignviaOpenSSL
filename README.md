## How to create self signed with openssl

OpenSSL is a free utility that comes with most installations of MacOS X, Linux, the *BSDs, and Unixes. You can also download a binary copy to run on your Windows installation. And OpenSSL is all you need to create your own private certificate authority. The process for creating your own certificate authority is pretty straight forward:
- Create a private key
- Self-sign
- Install root CA on your various workstations
Once you do that, every device that you manage via HTTPS just needs to have its own certificate created with the following steps:
- Create CSR for device
- Sign CSR with root CA key

openssl genrsa -out RootCA.key 4096
openssl req -config openssl.cnf -x509 -new -nodes -key RootCA.key -sha256 -days 50000 -out RootCA.pem -subj "/C=UA/ST=UA/L=UA/O=UA Inc./OU=IT Department/CN=UA.com"

openssl genrsa -out Devide.key 4096
openssl req -config openssl.cnf -new -key Devide.key -out Devide.csr -subj "/C=UA/ST=UA/L=UA/O=UA Inc./OU=IT Department/CN=UA.com"

openssl x509 -req -in Devide.csr -CA RootCA.pem -CAkey RootCA.key -CAcreateserial -out Devide.crt -days 50000 -sha256 -extfile v3.ext 

openssl pkcs12 -export -out Devide.pfx -inkey Devide.key -in Devide.crt

dhparam if need
openssl dhparam -dsaparam -out dhparam.pem 2048

display cert in text
openssl x509 -in RootCA.pem -text

openssl.exe pkcs12 -info -in Devide.pfx | openssl.exe x509 -noout -text > Devide.pfx.details.txt

openssl pkcs12 -in Devide.pfx -out Devide.pem -nodes

