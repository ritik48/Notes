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

---

Above we setup the server block in the `nginx.conf`, but we can also use sites-enabled and sites-available folder that is present inside the directory /etc/nginx to configure different servers.

### Complete step hosting a MERN app.

We have domain: `example.com`

let's say we have frontend code in `/usr/frontend` and backend in `/usr/backend`

1. First we will `build` our react app.

   ```
   cd /usr/frontend
   npm run build
   ```

   This will generate a build folder having all the files required to run the app.
   
   Now, we will serve this using nginx

2. Setup server block in `/etc/nginx/sites-available`
   
   currently here we have a file default, it has a sevrer block that serves a default page on port 80
   
   so we will remove it, and create a file with any name, but convention is to use domain name (example.com)

   now, in `example.com`

   ```
   server {
           listen 80;
           server_name example.com www.example.com;
          
           root /usr/frontend/build # this build will be served when domain example.com or www.example.com is visited
           
           index index.html;

           location / {
                try_files $uri ./index.html
           }
   }
   ```
   In the above server block, we have a location block which specifie what will happen when `/` route is hit. Here, we give two option `$uri` or `./index.html`
   
   If our url is `http://examp.com/about`, then here `$uri` is `/about.html`
   
   `location` block will first try to find if we have `about` file. if not then it will just serve `index.html`
   
   Serving `index.html` here is crucial for our react app, as the routing in our react is done using react-browser router, there is no actual navigation it's just that react-browser-router changes the component based on url provided.

   The setup will serve frontend.

   Now, in our frontend we will be having api calls to nodejs backend, so we have to specify here how to handle that.
   
   Let's say from frontend we make request to backend `http://localhost:3000/api`

   Now, we need a way to differentiate which request is backend and which frontend.

   Let's have a convention, if url has `/api` appended to it it means the requets needs to go to the backend.
   
   So, we will setup a server block which will proxy the request to this backend endpoint when `/api` route is visited.

   So, we will have one more `location` block.

   Complete code:

   ```
   server {
           listen 80;
           server_name example.com www.example.com;
          
           root /usr/frontend/build # this build will be served when domain example.com or www.example.com is visited
           
           index index.html;

           location / {
                try_files $uri ./index.html
           }
           location /api {
                proxy_pass: http://localhost:3000  (backend endpoint)
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
           }
   }
   ```
   So, now when we hit `example.com/api/projects`, it will proxy the request to `http://localhost:3000` and the resulting url will be `http://localhost:3000/api/projects`

  3. Setup `site-enabled` folder in `/etc/nginx/`

     This directory contains symbolic links to the configuration files in sites-available that you want to enable.
     
     **Why Symbolic Links?:** 
    
     Instead of copying configuration files from sites-available to sites-enabled, Nginx uses symbolic links. This makes it easier to enable or disable sites by simply adding or removing     these links, without modifying the actual configuration files.

     ***Only the configurations that have symbolic links in sites-enabled will be used by Nginx.***

     So, to setup symbolic links for the websites defined in the `sites-available`, run the following command.

     `sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/`

     Now, save the file and test the nginx status.

     `sudo nginx -t`

     if there are errors, then reload the nginx server

     `sudo nginx -s reload`

     So, our frontend is ready.

     Now, we will start our backend.

4. Start the backend

   To start a backend and keep it running we will use process manager `pm2`

   Install:

   `npm i -g pm2`

   now, inside the backend directory

   `pm2 start npm --name "backend" run dev`

    Explanation:
    pm2 start: This tells PM2 to start a new process.

    npm: PM2 is running the npm command.

    --name "backend": This gives the PM2 process a name (backend) to identify it easily in the list of PM2 processes. It’s important to put the name in quotes if it contains spaces or special characters.

    --: The -- is used to pass arguments to the command (npm in this case). It tells PM2 that any following arguments should be passed to npm, not to pm2 itself.

    run dev: This is the npm command to run the dev script from your package.json.

   


     

