# ec2-node-app-deploy
## wut did i write?
This read me holds the basic steps my stuipid brain 🧠 will definitely forget about the steps need to be followed for making node project deployed, and serving the RESTful APIs, so here it is....(you're welcome future 🔮 me!)


# How to Deploy node App on ec2 Instance

Dependencies :
[![flat-square](https://shields.io/badge/NGINX-white?logo=NGINX&logoColor=success&style=flat-square)](https://www.nginx.com) 	[![flat-square](https://shields.io/badge/Git-white?logo=GIT&style=flat-square)](https://git-scm.com/)	[![flat-square](https://shields.io/badge/PM2-white?logo=PM2&logoColor=2B037A&style=flat-square)](https://pm2.keymetrics.io)	[![flat-square](https://shields.io/badge/Node.Js-white?logo=Node.js&style=flat-square)](https://nodejs.org)	[![flat-square](https://shields.io/badge/nvm-white?&style=flat-square)](https://github.com/nvm-sh/nvm)	[![flat-square](https://shields.io/badge/MongoDb-white?logo=MongoDb&style=flat-square)](https://www.mongodb.com)

#### Notations
[i] denotes info

`denotes command`
	
#### Steps	
 - Create EC2 instance
 - [ i ] Install nvm
	 - `wget -qO- [https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh](https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh) | bash`
	 - [ i ] open terminal and verify
		 - `command -v nvm`
		 - or `nvm --help`
 - [ i ] Install git
	 - `sudo apt-get install git`
 - [ i ] Install node
	 - `nvm ls-remote`
	 - `nvm install 14.16.0` (prefer to use your locally installed node version here...)
	 - [ i ] Verify the installations the current versions of node and nvm
		 - `node --version`
		 - `npm --version`
 - [ i ] Install Mongodb
	 - `wget -qO - [https://www.mongodb.org/static/pgp/server-4.4.asc](https://www.mongodb.org/static/pgp/server-4.4.asc) | sudo apt-key add -`
	 - `echo "deb [ arch=amd64,arm64 ] [https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.4](https://repo.mongodb.org/apt/ubuntu%20bionic/mongodb-org/4.4) multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list`
	 - `sudo apt-get update`
	 - `sudo apt-get install -y mongodb-org` (Install mongodb)
	 - `sudo systemctl start mongod` (start stand alone mongod instance)
	 - `sudo systemctl status mongod` (chect status for mongod instance)
 - [ i ] clone any repo-project from git
	 - `npm i`
	 - [ i ]  create .env file
		 - `vi .env` or `sudo touch .env` and `sudo nano .env`
		 - `past env properties`
		 - for vi editor 
			 - hit `esc` 
			 - write `:wq` and hit `Enter`
		 - for nano editor 
			 - hit `ctrl+o` `Enter` `ctrl+x`
 - [ i ] Install PM2
	 - `npm install pm2@latest -g`
	 - `pm2 status` (verify the installation)
	 - `pm2 start server.js --name "api" --watch --ignore-watch="node_modules"` (in current project dir. with server.js)
	 - [ i ] check the api service id
		 - `pm2 status`
	 - `pm2 reload *service id here*`
	 - `pm2 logs *service id here*` ( to check if the node app is running and connected with db or shows error)
	 - [ i ] to delete the project **⚠ do not follow this step**
		 - `pm2 delete 0`  (to remove running project)
 - [ i ] Install nginx
	 - `sudo apt update`
	 - `sudo apt install nginx`
	 - `sudo service nginx status`
	 - `sudo nano /etc/nginx/sites-available/default`  (press `alt+shift+#` to see line numbers)
	 - `sudo service nginx restart`


nginx config file

> With assumption of node app running on port 6000

       
    # Default server configuration
    #
    server {
    	listen 80 default_server;
    	listen [::]:80 default_server;
    
    # if server domain is available for you. write next line as server_name example.com;
        #server_name example.com;
    	server_name _;
    
    	location /api {
    		# First attempt to serve request as file, then
    		# as directory, then fall back to displaying a 404.
    		#try_files $uri $uri/ =404;
    		proxy_pass http://127.0.0.1:6000;
    	}
    }


