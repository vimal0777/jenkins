# Install Jenkins on AWS EC2 Ubuntu 16.04
 Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.



### Prerequisites
1. EC2 Ubuntu Server 16.04 LTS  Instance [Get help here](https://www.youtube.com/watch?v=KDtS6BzJo3A)
   - With Internet Access
   - Security Group with Port `8080` open for internet
1. Java v1.8.x 

## Install Java
We will be using open java, Get latest version from http://openjdk.java.net/install/
```sh
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update

sudo apt-get install openjdk-8-jdk
```

### Confirm Java Version
Lets install java and set the java home
```sh
java -version
find /usr/lib/jvm/java-1.8* | head -n 3 
``` 
above command execution will displays a path like (```sh /usr/lib/jvm/java-1.8.0-openjdk-amd64```) and copy that displays path 

## Edit Bash Profile in vim editor
```sh
vi .bashrc
```
edit

```sh
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
PATH=$PATH:$JAVA_HOME:$HOME/bin

export PATH

# To set it permanently update your .bash_profile
source ~/.bashrc
```

Check path by executing below command
```sh
echo $JAVA_HOME
echo $PATH

```

_The output should be something like this,_
```
[root@~]# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```

## Install Jenkins
You can install jenkins using the rpm or by setting up the repo. We will setup the repo so that we can update it easily in future.
Get latest version of jenkins from https://pkg.jenkins.io/redhat-stable/
```sh
sudo apt-get install wget
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
sudo apt-get update

rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum -y install jenkins
```

### Start Jenkins
```sh
# Start jenkins service
systemctl start jenkins

# Setup Jenkins to start at boot,
systemctl enable jenkins
```

#### Accessing Jenkins
By default jenkins runs at port `8080`, You can access jenkins at
```sh
http://YOUR-SERVER-PUBLIC-IP:8080
```
#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
  - Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
  - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Create another admin user id

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under Build section
	Execute shell : echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"

### Next Step
- [x] [Configure Users & Groups in Jenkins](https://youtu.be/jZOqcB32dYM)
- [x] [Secure your Jenkins Server](https://youtu.be/19FmJumnkDc)
- [x] [Jenkins Plugin Installation](https://youtu.be/p_PqPBbjaZ4)
- [x] [Jenkins Master-Slave Configuration](https://youtu.be/hwrYURP4O2k)
- [ ] Setup Jenkins to run inside Tomcat Server
