### **Steps to Set Up HTTPS for Your Domain Using Nginx and Certbot**

---

 What is Nginx?
 
Nginx is a powerful web server. It can:

Serve your website's HTML, CSS, and images

Work as a reverse proxy, forwarding traffic to backend applications (like Django)

Handle HTTPS (SSL) encryption

Act as a load balancer or cache

We’ll use Nginx to send browser requests to your Django app and to manage HTTPS using SSL certificates.

---

### **1. Install Nginx**

First, you need to install Nginx, a web server that will handle your site:

```bash
sudo apt update
sudo apt install nginx -y

////////////////////////////////////////////////////////////////////////////
2. Configure Nginx for Your Domain
Next, you need to set up Nginx to serve your website using HTTPS. You’ll create a configuration file for your domain.

Open the Nginx configuration file:
sudo nano /etc/nginx/sites-available/djangoapp

////////////////////////////////////////////////////////////////
Add the following content to this file:
server {
    listen 443 ssl;
    server_name manikantadevops.hopto.org;

    ssl_certificate /etc/letsencrypt/live/manikantadevops.hopto.org/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/manikantadevops.hopto.org/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

This configures Nginx to listen on port 443 (HTTPS) and forward requests to your Django app running on port 8000.
Save the file and exit.

//////////////////////////////////////////
3. Enable the Configuration
Now, you need to link your configuration file to the sites-enabled directory, so Nginx can use it:

sudo ln -s /etc/nginx/sites-available/djangoapp /etc/nginx/sites-enabled/

Then, remove the default configuration:
sudo rm /etc/nginx/sites-enabled/default

/////////////////////////////////////////////////////////////////

4. Test and Restart Nginx
Check if the Nginx configuration is correct:
sudo nginx -t
If it’s correct, restart Nginx to apply the changes:
sudo systemctl restart nginx

///////////////////////////////////////

5. Allow HTTP/HTTPS Traffic in the Firewall
Make sure that your server allows traffic on HTTP and HTTPS ports:
sudo ufw allow 'Nginx Full'
sudo ufw enable
This ensures that your server can accept requests on both port 80 (HTTP) and port 443 (HTTPS).

6. Install Certbot and Obtain SSL Certificate
Now, you need to get an SSL certificate to secure your site. First, install Certbot, which will automatically get and configure the SSL certificate for your domain:
sudo apt install certbot python3-certbot-nginx -y

Next, run Certbot to get the certificate for your domain:
sudo certbot --nginx -d manikantadevops.hopto.org
Certbot will ask for your email address, agree to the terms, and then it will obtain and install the SSL certificate.

7. Test and Reload Nginx
Once the changes are made, test the configuration:
sudo nginx -t
If everything is okay, reload Nginx to apply the changes:
sudo systemctl reload nginx





