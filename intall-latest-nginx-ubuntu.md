# How to setup latest version of Nginx on ubunutu

Run these commands from terminal:

```
sudo bash -c 'echo "deb https://nginx.org/packages/ubuntu/ focal nginx" >> /etc/apt/sources.list'
sudo bash -c 'echo "deb-src https://nginx.org/packages/ubuntu/ focal nginx" >> /etc/apt/sources.list'
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD827BD9BF62
sudo apt update 
sudo apt install nginx
sudo systemctl start nginx
```

Nginx will be installed inside /etc/ directory.

