# Installation of Laravel Homestead in MacOS

Install Virtual Machine provider such as VirtualBox/VMware/Parallel

1. Install Vagrant
2. Install The Homestead Vagrant Box by following command:

    `cd ~`
    `git clone https://github.com/laravel/homestead.git Homestead`

3. Run `bash init.sh` on terminal to create the `Homestead.yaml` configuration file.
4. After generate `Homestead.yaml` file, open it and you can see the `provider` key and `ip` key indicates that which provider and IP Address should be used.
5. The ```folders``` property lists all of the folders you wish to share with your Homestead environment. Change the value to your directory which the source code of the projects corresponding to key "map" and add the key "type" for the synced folder configuration as the following example:
```yml
    folders:
        - map : <Your directory as you desire>
        to : /home/vagrant/Code
        type : "nfs"
```
6. Change the "sites" property as follows:
```yml
    sites :
        - map : <Your domain as you desire>
          to : /home/vagrant/Code/public
```
7. The `databases` property indicate which database used in the projects
8. Run `sudo vi /etc/hosts` on your local shell to open the host file
9. Add `192.168.10.10   example.test` and save file for old version. "192.168.56.56" for new version.
10. Set the `ssh-keygen` to access homestead as follows:

``` 
ssh-keygen -t rsa -C ""
```

11. After updating the `Homestead.yaml` and host file, be sure to re-provision the machine by running `homestead reload -provision`
12. Run `homestead up` to start up the virtual machine
13. Access "http://example.test" via your web browser, you can see the page.

Remark: For development environment, the domain name should be like `http://<HOST_NAME>.test`

## 4 common use for homestead

1. Start up Homestead:  ```homestead up```
2. Enter Homestead: ```homestead ssh```
3. Pause Homestead: ```homestead suspend```
4. Shutdown Homestead: ```homestead halt```
