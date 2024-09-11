### Deploying Express app on EC2

1.  Create an EC2 instance
2.  Generate .pem file to get access to this ec2 instance
3.  Click create instance

Above will get you a EC2 instance running.

Now, Open Custom TCP connection and add ports on which your express server is listening on
-  3000 (Ipv4)
-  3000 (Ipv6)

After this, go to your terminal and connect to this EC2 instance, using the below command:

`ssh -i your-key.pem ubuntu@your-instacne-ip`

With this you will be able to access the EC2 instance from your terminal. Now, you can run all the linux commands here.

### Running express project

1.  sudo apt update
2.  Install node in your ec2 instance
    -  Install nvm (Node version manager)

       `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash`
    - Now, install any node version you want

      `nvm install 20` (installs node version 20)

     and thats it. you have nodejs in your ec2 instance.

3.  Clone your repository in this EC2 instance.
4.  and follow that usual commands that you use to run express app in your system

If you had setup the ports as said in the above step, you would be able to access your express app on **http://your-ec2-ip:3000**

### Configuring Nginx

NGINX is open-source web server software used for **reverse proxy**, **load balancing**, and **caching**.

We will be using it as reverse proxy.

Currenlty, we have to specify the port while accessing our express app, but we can change this using Nginx.

1.  Install Nginx in your EC2 instance

    sudo apt install nginx

2.  To know if it has been installed, access your app on the ip without mentioning any port and you will see below page

     (http://your-ec2-ip)

    ![image](https://github.com/user-attachments/assets/6c6228c6-b517-489b-baf8-ab3a65bd14cd)

    Basically every site by default listens on the port 80, and this is where Nginx also listens to.
    Now, there is a configuration file which we need to edit, to forward the request that comes to port 80, to the port where our express app is running (here 3000)

    that file is: /etc/nginx/nginx.conf

    Use this command to edit the file: `sudo nano /etc/nginx/nginx.conf`

    here write down the follwoing code:
```
    events {
    
      # events directives
    
    }
    http {
          server {
          listen 80;
          server_name _; OR server_name yourrdomain.com

          location / {
                proxy_pass: http://localhost:3000  (the port where you want the Nginx to redirect when your particular domain (mentioned above) is hit.
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
          }        
          } 
    }
```
And save this file.

Before applying the changes, ensure your configuration file is correct.

1.  `sudo nginx -t` 
     If there are no errors, proceed
2.   Reload Nginx to apply the changes.

    `sudo nginx -s reload`
  
Now, you will be able to access your application without specifying the port.

#### Note: 

If you don't want to specify server_name with your_domain_or_ip, you can use the underscore (_) as a wildcard to accept all incoming requests, regardless of the domain or IP. Here's how you can modify the server block:

When you configure `server_name _`; in Nginx, it means that the server block will handle requests from any domain or IP address. Here's what that implies:

**Any Domain Name:** If someone accesses your server using any domain name (like example.com, myapp.com, etc.), Nginx will accept the request and forward it to your React app (or whatever is being served). It doesn’t matter if the domain is actually set up to point to your server or not.

**Any IP Address:** It also means that any request made directly to the IP address of your EC2 instance will be accepted. For example, accessing http://<EC2_IP>/ will work even without a domain name pointing to the server.

      

[Important Chatgpt Notes](https://chatgpt.com/share/477c7fad-c02f-4e25-8bdd-4a4926cbd644)

