## this is a quick start setup and deploy of Openwhisk using docker-compose , however you can use kubernates and other tools to deploy
## basic setup

```
  $ sudo apt update # apt update
  $ sudo apt upgrade
  
  $ sudo apt install openjdk-11-jdk # java setup
  
  $ curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash - $ sudo apt-get install -y nodejs # nodejs setup
  
  
```
## docker setup

```
$ sudo apt remove docker docker-engine docker.io containerd runc

$ sudo apt-get install \ # setup the tools needed
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add # official GPG key 

$ sudo add-apt-repository \ # add stable repo
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \ 
   $(lsb_release -cs) \
   stable
 
$ sudo apt install docker-ce docker-ce-cli containerd.io # setup docker
```
## docker-compose setup

```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-Linux-x86_64" -o /usr/local/bin/docker-compose # download and you can use a version you like
$ sudo chmod +x /usr/local/bin/docker-compose # add authority
```
## download docker compose deploy related tools
```
$ mkdir ~/Openwhisk
$ cd ~/Openwhisk

$ git clone https://github.com/apache/openwhisk.git
$ git clone https://github.com/apache/openwhisk-devtools.git
git clone https://github.com/apache/openwhisk-catalog.git


wget https://github.com/apache/openwhisk-cli/releases/download/1.1.0/OpenWhisk_CLI-1.1.0-linux-amd64.tgz
$ mkdir openwhisk-cli
$ tar zxvf OpenWhisk_CLI-1.1.0-linux-amd64.tgz -C ./openwhisk-cli
$ cp openwhisk-cli/wsk openwhisk/bin/wsk
$ rm OpenWhisk_CLI-1.1.0-linux-amd64.tgz
```
## use docker compose to deploy wsk 
```
$ cd ~/Openwhisk/openwhisk-devtools/docker-compose/
$ vim make.props
  OPENWHISK_PROJECT_HOME=/root/Openwhisk/openwhisk
  OPENWHISK_CATALOG_HOME=/root/Openwhisk/openwhisk-catalog
  WSK_CLI=/root/Openwhisk/openwhisk-cli/wsk
$: sudo ln -sf /bin/bash /bin/sh # using bash instead dash (which is the default of Ubuntu)
$ cd ~/Openwhisk/openwhisk-devtools/docker-compose/
$ sudo make $(cat ./make.props) quick-start
```
## configure wsk
```
$ cd ~/Openwhisk/openwhisk-cli/
$ wsk property set --auth AUTH
$ wsk property set --apihost  APIHOST 

```
above AUTH and APIHOST are saved in a hiding file in ~/Openwhisk/openwhisk-devtools/docker-compose/.wskprops

## set environment variable wsk
```
vim /etc/profile
PATH=$PATH:/root/Openwhisk/openwhisk/bin
source /etc/prifile
```

after these, you can use 'wsk' comment anywhere such as:

```
root@openwhisk:~/Openwhisk# wsk

        ____      ___                   _    _ _     _     _
       /\   \    / _ \ _ __   ___ _ __ | |  | | |__ (_)___| | __
  /\  /__\   \  | | | | '_ \ / _ \ '_ \| |  | | '_ \| / __| |/ /
 /  \____ \  /  | |_| | |_) |  __/ | | | |/\| | | | | \__ \   <
 \   \  /  \/    \___/| .__/ \___|_| |_|__/\__|_| |_|_|___/_|\_\
  \___\/ tm           |_|

Usage:
  wsk [command]

Available Commands:
  action      work with actions
  activation  work with activations
  package     work with packages
  rule        work with rules
  trigger     work with triggers
  sdk         work with the sdk
  property    work with whisk properties
  namespace   work with namespaces
  list        list entities in the current namespace
  api         work with APIs
  project     The OpenWhisk Project Management Tool

Flags:
      --apihost HOST         whisk API HOST
      --apiversion VERSION   whisk API VERSION
  -u, --auth KEY             authorization KEY
      --cert string          client cert
  -d, --debug                debug level output
  -h, --help                 help for wsk
  -i, --insecure             bypass certificate checking
      --key string           client key
  -v, --verbose              verbose output

Use "wsk [command] --help" for more information about a command.
```











