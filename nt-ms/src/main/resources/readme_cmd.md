Create A Self Signed Client Cert
We will use key tool command for this.
keytool -genkeypair -alias nt-gateway -keyalg RSA -keysize 2048 -storetype JKS -keystore nt-gateway.jks -validity 3650 -ext SAN=dns:localhost,ip:127.0.0.1 

Create Self Signed Server Cert:
keytool -genkeypair -alias nt-ms -keyalg RSA -keysize 2048 -storetype JKS -keystore nt-ms.jks -validity 3650 -ext SAN=dns:localhost,ip:127.0.0.1

Create public certificate file from client cert:
Now that we’ve client and server certs created, we need to set up trust between both. To do that, we’ll import client cert in to the server’s trusted certificates and vice versa. But before we can do that, we need to extract public certificate of each jks file.
keytool -export -alias nt-gateway -file nt-gateway.crt -keystore nt-gateway.jks
Enter keystore password:
Certificate stored in file <nt-gateway.crt>

Create Public Certificate File From Server Cert:
keytool -export -alias nt-ms -file nt-ms.crt -keystore nt-ms.jks
Enter keystore password:
Certificate stored in file <nt-ms.crt>

Import Client Cert to Server jks File:
keytool -import -alias nt-gateway -file nt-gateway.crt -keystore nt-ms.jks

Import Server Cert to Client jks File:
keytool -import -alias nt-ms -file nt-ms.crt -keystore nt-gateway.jks

