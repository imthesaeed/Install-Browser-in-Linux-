# Install-Browser-in-Linux-
Chromium is an open-source browser project that aims to build a safer, faster, and more stable build by Google

    You can easily access a browser in your non-gui Linux server
    You can easily run your Node Extensions

Install Docker
~~~ruby
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Docker version check
docker --version
~~~
check timezone 
~~~ruby
realpath --relative-to /usr/share/zoneinfo /etc/localtime
~~~

Install Chromium
1. Create directory
~~~ruby
mkdir chromium
cd chromium
~~~
2. Create docker-compose.yaml file
~~~ruby
nano docker-compose.yaml
~~~
3. Paste the following code in it

    CUSTOM_USER & PASSWORD: Replace your favorite credentials to login to chromium
    TZ: Replace with your server timezone
    CHROME_CLI: The main page when you open browser
    ports: You can replace 3010 & 3011 if they have conflict
~~~ruby
---
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - CUSTOM_USER=     #Replace username
      - PASSWORD=    #Replace password
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - CHROME_CLI=https://github.com/0xmoei #optional
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   #Change 3010 to your favorite port if needed
      - 3011:3001   #Change 3011 to your favorite port if needed
    shm_size: "1gb"
    restart: unless-stopped
~~~


    To save and exit: Ctrl+X+Y+Enter


Stop and Delete Chromium
~~~ruby
docker stop chromium
docker rm chromium
docker system prune
~~~



