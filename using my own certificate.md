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

After a short while after *Press Enter to Continue*


Press Enter to Continue
```
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/lowcode.dougeyebeem.se/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/lowcode.dougeyebeem.se/privkey.pem
This certificate expires on 2023-09-06.
These files will be updated when the certificate renews.

NEXT STEPS:
- This certificate will not be renewed automatically. Autorenewal of --manual certificates requires the use of an authentication hook script (--manual-auth-hook) but one was not provided. To renew this certificate, repeat this same certbot command before the certificate's expiry date.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
The information to be entered into the Domain Mapping form is in the fullchain.pem and privkey.pem files.

If you need to use a certificate coming from a customer you will probably receive it as a .pfx file
You can extract the needed information using the following commands:
```
openssl pkcs12 -in <<infil.pfx>> -nocerts -out <<domainname>>_keyfile-encrypted.key  
	or
openssl pkcs12 -in <<infil.pfx>> -nocerts -nodes -out <<domainname>>_keyfile-not-encrypted.key

openssl pkcs12 -in <<infil.pfx>> -clcerts -nokeys -out <<domainname>>_certificate.crt

openssl pkcs12 -in <<infil.pfx>> -cacerts -nokeys -chain -out ca_<<domainname>>_certificate.crt
```

The output files here should basically have the same type of information as the fullchain.pem and the privkey.pem files


Last step is to do the actual Domain Mapping:
The certificate entry should have the full chain, ie three certificates
![domainMappingForm](/images/domainMappingForm.png)

when done it should look similar to this
![domainMappingDone](/images/domainMappingDone.png)


