
# Create self-signed certificate without CSR (certificate signing request)#

link: `http://dracoblue.net/dev/https-nginx-with-self-signed-ssl-certificate/188/`


Create directory to store certificate 
	`mkdir /opt/nginx/ssl && cd /opt/nginx/ssl`

Create certificate with private key
	`openssl req -new -x509 -nodes -out server.crt -keyout server_with_pass_phrase.key`

Since nginx needs to access the key, we need to unlock private key without pass phrase.
	`sudo openssl rsa -in server_with_pass_phrase.key -out server.key`
	`chmod 600 server.key`

config for nginx

```javascript
server {
    listen               443;
    ssl                  on; 
    ssl_certificate      /opt/nginx/ssl/server.crt;
    ssl_certificate_key  /opt/nginx/ssl/server.key;
    server_name ssl.example.org;
    location / {
            root /var/www/ssl.example.org;
            index index.php;
    }   
    # ... and so on
}
```

# Create certficate with CSR #

link: `https://www.digitalocean.com/community/articles/how-to-create-a-ssl-certificate-on-nginx-for-ubuntu-12-04`


