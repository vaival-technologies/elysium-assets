# How to setup blockscout with elysium

```
sudo apt -y update && sudo apt -y upgrade
```

## Add erlang repos
```
# go to your home dir
cd ~
# download deb
wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb
# download key
wget https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc
# install repo
sudo dpkg -i erlang-solutions_2.0_all.deb
# install key
sudo apt-key add erlang_solutions.asc
# remove deb
rm erlang-solutions_2.0_all.deb
# remove key
rm erlang_solutions.asc
```

## Add NodeJS repo
```
sudo curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```
