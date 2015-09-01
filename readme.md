#Symfony 2 & OroCRM Vagrant box (for Windows 7 64)
I made this box via puphpet.com (base box 'puphpet/ubuntu1404-x64') and tested on several machines.
It's almost a standard config but i solved trubles:

 - Issue with php-fpm and non-ascii characters
 - Issue when vagrant can't find an ssh client
 - Issue with vhost configuration for symfony projects
 
LAMP contents:

 - PHP 5.6
 - Apache 2.4
 - MySQL 5.6
 
All these and another modules you can change via [puphpet.com](https://puphpet.com) simply by dragging config.yaml into the browser window
 
##1. Prerequisites
Before running this vagrant box you have to install these apps:

 - [VirtualBox](https://www.virtualbox.org/) (4.3.x)
 - [Vagrant](https://www.vagrantup.com/) (1.7.4 or higher)
 - [CygWin](https://www.cygwin.com) (with ssh utilities)

##2. Windows %PATH% env variable settings
After all software was installed add path to CygWin dir to Windows %PATH% variable:
```sh
set PATH=%PATH%;C:\cygwin64\bin
```
or 
```sh
setx /M PATH "%PATH%;C:\cygwin64\bin"
```
or (the guaranteed way):

 - From the desktop, right-click My Computer and click Properties.
 - In the System Properties window, click on the Advanced tab
 - In the Advanced section, click the Environment Variables button.
 - Finally, in the Environment Variables window (as shown below), highlight the Path variable in the Systems Variable section and click the Edit button. Add "C:\cygwin64\bin" to the path line
 
##3. Installation and run
Clone this repo and simply run:
```sh
vagrant up
```

##4. Symfony project setting
To avoid permission problems and speed up your app add these methods to your app\AppKernel.php:
```sh
public function getCacheDir()
{
    if (in_array($this->environment, array('dev', 'test'))) {
        return '/home/vagrant/symfony/cache/' . $this->environment;
    }
	
    return parent::getCacheDir();
}

public function getLogDir()
{
    if (in_array($this->environment, array('dev', 'test'))) {
        return '/home/vagrant/symfony/logs';
    }

    return parent::getLogDir();
}
```

##5. MySQL remote connection credentials
You can connect to MySQL using any app that supports SSH tunneling. I.e. i use SQLyog

```sh
SSH Host Address: 192.168.56.101 (you can change it in the config.yaml)
Username: vagrant
SSH Port: 22
Passphrase: vagrant
Private key: choose your own at puphpet\files\dot\ssh\id_rsa.ppk 

--------

MySQL Host Address: 127.0.0.1
Username: oquser (you can change it in the config.yaml)
Password: oq7382 (you can change it in the config.yaml)
Port: 3306
```

##6. Edit Windows hosts file
Open file C:\Windows\System32\drivers\etc\hosts and add a string:
```sh
192.168.56.101 symphpet.dev
```
Both parameters (ip-address and hostname) can be changed in config.yaml

##7. Test
Open browser and enter the url symphpet.dev (you can change host address in config.yaml), you should see a text "Hello, Symphpet!"
