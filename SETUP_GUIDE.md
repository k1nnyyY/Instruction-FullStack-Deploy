Some changes in your projects...
Don't forget change domain in cookie settings

//1 - Add new root user
adduser os

open file:
  visudo

Add new row "os ALL=(ALL:ALL)ALL" after "root ALL=(ALL:ALL) ALL"

switch with command "su - tom"
test with command "sudo apt-get update"

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//2 - Install nvm, node, npm

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash

nvm -v

(install same version as in your PC)
nvm install node 21.6

test with commands "node -v" or "npm -v"

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//3 - Git pull and push

git add .
git commit -m 'Init'
git add origin
git push origin main


// Create key for git in server

ssh-keygen -o -t rsa -C “ssh@github.com”
"ll" - for test
"cat id_rsa.pub" //copy

paste to github account

mkdir "project-folder"

sudo apt-get install git-all

//change to your git url
git clone git@github.com:username/front.git
git clone git@github.com:username/back.git

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//4 - Database

sudo apt update
sudo apt install postgresql postgresql-contrib

sudo -u postgres psql

// Please change db name and username

CREATE ROLE os WITH LOGIN PASSWORD '123456' CREATEDB;
CREATE DATABASE k1nnyyY OWNER os;
GRANT ALL PRIVILEGES ON DATABASE k1nnyyY TO os;

// list of users
\du

// list of dbs
\l

// Exit
\q

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//5 - Env and install dep

create .env back and front

NODE_ENV = production
DATABASE_URL = postgresql://os:123456@localhost:5432/k1nnyyY?schema=public
JWT_SECRET = FedDrfg&#

// (not reaquired) if you need with pnpm or yarn
npm install -g pnpm
npm install -g yarn

yarn or pnpm install or npm install

// prisma for back
npx prisma db push (only first lunch after use "npx prisma migrate deploy")

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//6 - PM 2 (process manager)

npm run build

npm install pm2 -g

// for npm
pm2 start npm --name client -- start
pm2 start npm --name server -- start

// for yarn
pm2 start yarn --name client -- start
pm2 start yarn --name server -- start

// for pnpm
pm2 start pnpm --name client -- start
pm2 start pnpm --name server -- start


//autolunch
pm2 startup ubuntu

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//7 - Nginx (web-server)

sudo apt-get install nginx

sudo nano /etc/nginx/sites-available/default

server {
  location /api {
        proxy_pass http://localhost:4200;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

location /{
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
		}

//for test
sudo nginx -t

sudo service nginx restart

//if u have statics folders
location /public {
    include /etc/nginx/mime.types;
    root /home/....;
}

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//8 - Bonus SSL

sudo apt install certbot python3-certbot-nginx
sudo systemctl reload nginx

//change domain
sudo certbot --nginx -d test.com

sudo systemctl status certbot.timer

//check for errors
sudo certbot renew --dry-run

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

If u update files, you should on server:


git pull && pnpm run build && pm2 reload all