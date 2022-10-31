# Ubuntu/Kali/Zorin setup with proxy environment
Once you've installed your `os` of debian distribution you need to take not of the following.

If you are operating in a proxy environment, there are two layers of proxy, i.e `root`, `session` and `user` level proxy
For user level we can add proxy in the `/etc/environment` file where it can be read by some applications or use the network GUI

`Ubuntu: Settings > Network > Network Proxy > Manual` path

This applies for `kde plasma` and `gnome` desktop environment

![filling-out-proxy-info-in-ubuntu-gui-proxy-settings](https://user-images.githubusercontent.com/55891685/199001021-4097d50c-228b-4289-9889-e35d49011ac4.png)

Set appropriate variables based on your organization details

If you are using a setup file you can add the following if you prefer session based in `bash` , `sh` , `zsh` ... e.t.c
 ```sh
http_proxy="http://<username>:<password>@<hostname>:<port>/"
https_proxy="http://<username>:<password>@<hostname>:<port>/"
ftp_proxy="http://<username>:<password>@<hostname>:<port>/"
no_proxy="<pattern>,<pattern>,...
```
e.g
```
http_proxy="172.16.63.17:3128/"
```
or for bash sessions you can do an export and once that session is closed you can just exit and proxy will be gone
```sh
export http_proxy="http://<username>:<password>@<hostname>:<port>/"
export https_proxy="http://<username>:<password>@<hostname>:<port>/"
export ftp_proxy="http://<username>:<password>@<hostname>:<port>/"
```
If you don't like this kind of redundancy you can make use of a function for your `$SHELL` e.g 
- `.bashrc` for bash
- `.zshrc` for zsh
e.t.c

```
...sh
#Export proxy
function setProxy(){
export {http,ftp,https}_proxy="http://<username>:<password>@<hostname>:<port>/"
export {HTTP,FTP,HTTPS}_PROXY="http://<username>:<password>@<hostname>:<port>/"
}
# unset proxy
function unsetProxy(){
unset {http,ftp,https}_proxy
unset {HTTP,FTP,HTTPS}_PROXY
}
```

For the above the functions to set proxy now we'll just call them from the terminal i.e

```sh
setProxy
```
Should set proxy for any program that will be launched from that terminal via the live session eg `pnpm`,'npm',`vcode`

To set up the system to be able to download system remote repositories and updates you should consider the `etc` folder since this is where most of the configuration files are stored. If you are not sure always store a backup of a file you find in this folder to make sure if you mess up the files you have a restore point

For the `apt` , `apt-get` and `aptitude` package managers to be able to locate remote repositories via proxy we need to create a config file for this

To setup this file we need to create a file in the `/etc/apt/` folder known as `apt.conf`
```
sudo nano /etc/apt/apt.conf
```
In the file add the following proxy content
```sh
Acquire::http::proxy "http://<username>:<password>@<hostname>:<port>/"
Acquire::https::proxy "http://<username>:<password>@<hostname>:<port>/"
Acquire::ftp::proxy "http://<username>:<password>@<hostname>:<port>/"
```
Once you've added your correct values then you can update and install apps
```sh
sudo apt update
```
In the logs if you see `Ign:...` means network is unreachable but `Get:...` means success
To disable this just re-edit the file and add `#` before the `Acquire`

```sh
#Acquire::http::proxy "http://<username>:<password>@<hostname>:<port>/"
#Acquire::https::proxy "http://<username>:<password>@<hostname>:<port>/"
#Acquire::ftp::proxy "http://<username>:<password>@<hostname>:<port>/"
```

