# wd-stack
Wordpress Docker Stack with Traefik reverse proxy managing the network

## Purpose
This is two docker stacks. One is traefik, which will make it much easier to setup domains and networking in general. It will also do certs for your site so https should work automatically. The second is MySql, Wordpress, and FileBrowser. Filebrowser will make maintenance, debuging, configuring, developing, etc in wordpress a bit easier. It's not necessary, though if you don't like using command line, or want to manage your site all from the web, this makes it much more easier. 

## Requirements
- Linux Machine
-- I'm using Ubuntu 18.04.2 LTS
- Docker
- Docker Compose
- Think that's it

## Setup

- Go to wherever you want to install this i.e. `cd ~`
- Clone `git clone https://github.com/Sperovita/wd-stack.git newProjectFolderName`
- Goto new project `cd newProjectFolderName`
- Create a .env file in the wordpress folder `nano wordpress/.env`
- Copy below code, paste it into an editor if you want, edit applicable fields, then copy again, , paste into nano, for putty `right click`
```
DB_PASS=randomlyGeneratedPassword
DB_ROOT_PASS=anotherRandomlyGeneratedPassword
DB_USER=wordpress
DB_NAME=wordpress-db
WP_HOST_NAME=your.website.domain.pointing.at.ip.of.this.server
FB_HOST_NAME=your.website.domain.pointing.at.ip.of.this.server.with.a.different.subdomain.for.the.file.browser
COMPOSE_PROJECT_NAME=nameThatWillPrefixYourContainersAndVolumes
```
- Save `ctrl-x` `y` `enter`
- Add your email to Traefik for certs `nano traefik/config/traefik.toml`
- Save `ctrl-x` `y` `enter`
- Go into traefik `cd traefik`
- Spin up traefik `docker-compose up -d`
- Go into wordpress `cd ../wordpress`
- Spin up wordpress `docker-compose up -d`
- Go to FB_HOST_NAME and sign in user: admin, pass: admin
- Change the password
- Go to WP_HOST_NAME and setup your wordpress

## Notes

This is my first real docker project so it may be rough around the edges.

- The empty filebrowser.db file is needed for the bind to work correctly.
- The uploads.ini is necassary for you tu upload anything substabtial to wordpress
- There is some debate about whether any container that connects to the web being able to see the docker socks, from my understaning this makes it fall to traefik to keep their software secure. There are some workarounds in the traefik docs to create another layer of security if that's something you need, it's in a subject matter that is too time intensive for me to research at the moment. https://docs.traefik.io/configuration/backends/docker/#security-considerations
- If you do decide to spin up a second wordpress site you can clone this into another folder and delete traefik `rm -r traefik` or just leave it and only spin up the wordpress with different project names and host names (new passwords don't hurt) in the wordpress/.env file. You only need one traefik container for all your sites. I may create another repo for just the wordpress.
- I believe both docker compose files can be ran at version '2' if needed
- I may post links to docker-compose and docker installation instructions in the future, though those should be plentiful enough to google
