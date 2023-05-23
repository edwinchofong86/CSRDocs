To install a signed CSR (Certificate Signing Request) certificate on an Ubuntu server, you'll need the signed certificate file provided by the certificate authority (CA) and the private key that was generated when creating the CSR. Here's a step-by-step guide:

1. Connect to your Ubuntu server via SSH.

2. Copy the signed certificate file (often named something like `certificate.crt`) and the private key file (in this example, `private.key`) to a directory on your server. You can use `scp` or any other preferred method to transfer the files.

3. Move the certificate and private key files to the appropriate directory. Conventionally, you can place them in the `/etc/ssl/certs` and `/etc/ssl/private` directories, respectively. For example:
   ```
   sudo mv certificate.crt /etc/ssl/certs/
   sudo mv private.key /etc/ssl/private/
   ```

4. Set the appropriate permissions for the private key file to protect its confidentiality:
   ```
   sudo chmod 400 /etc/ssl/private/private.key
   ```

5. Update the SSL configuration file for the web server you're using (e.g., Apache or Nginx) to reference the certificate and private key. The location and name of the configuration file may vary depending on the web server you're using. Here are a couple of common examples:

   - For Apache:
     ```
     sudo nano /etc/apache2/sites-available/default-ssl.conf
     ```

     Locate the `<VirtualHost>` section for the SSL-enabled site, and within that section, update the following lines:
     ```
     SSLCertificateFile /etc/ssl/certs/certificate.crt
     SSLCertificateKeyFile /etc/ssl/private/private.key
     ```

   - For Nginx:
     ```
     sudo nano /etc/nginx/sites-available/default
     ```

     Within the server block, update the following lines:
     ```
     ssl_certificate /etc/ssl/certs/certificate.crt;
     ssl_certificate_key /etc/ssl/private/private.key;
     ```

   Note: Make sure to update the file paths to match the locations where you placed the certificate and private key files.

6. Save the configuration file and exit the editor.

7. Restart the web server to apply the changes. The command will depend on your web server. For example, for Apache:
   ```
   sudo service apache2 restart
   ```

   Or for Nginx:
   ```
   sudo service nginx restart
   ```

After restarting the web server, the signed certificate should be installed and actively used for SSL/TLS connections to your Ubuntu server.

Remember to renew the certificate before it expires to ensure uninterrupted secure connections. The renewal process may vary depending on your CA and any automation you have set up.


======================================================================
Self-Signed SSL Certificate creation command
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt

Add the below SSL configuration lines in virtual host configuration.
SSLEngine on
SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem
SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key

Apache modules to be enabled
a2enmod ssl
a2enmod headers
