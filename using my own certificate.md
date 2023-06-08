A description on how to use a custom domain name with a generated certificate

**Scenario**
create an application on IBM Cloud Engine.
create a subdomain for the new application
use Domain Mapping in Code Engine to be able to connect to the application using the subdomain

*here is the code engine information about the application*
![myCodeEngineApp.png](/images/myCodeEngineApp.png)

Create a sub domain for your application using a CNAME record with your domain registrator
My application in CodeEngine should run at lowcode.dougeyebeem.se
![myDNSsetting](/images/myDNSsetting.png)

Now I want to support https traffic and thus I need to have a valid certificate that I can import into the Domain Mapping functionality in Code Engine
I use certbot to generate a certificate, it will ask me to enter a challenge into my DNS in order to prove that I am in control of the domain for which I request a certificate. This certificate generation step can be done with many different tools in many different ways.

I use certbot
*sudo certbot certonly -d lowcode.dougeyebeem.se --manual --preferred-challenges dns*
got this response

```sh
Please deploy a DNS TXT record under the name:
_acme-challenge.lowcode.dougeyebeem.se.
with the following value:
vNRWYGJxN-uoOtnpiPq37AmLSEa8c-2vT8nrgKOOg8Y
```
![acmeChallenge record](/images/acmeChallenge.png)


