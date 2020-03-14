# Install Jenkins on AWS EC2 Ubuntu 16.04
 Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.



### Prerequisites
1. EC2 Ubuntu Server 16.04 LTS  Instance.
   - >1GB Ram (t2.micro)
   - Security Group with Port `8080` open for internet
1. Java v1.8.x 

## Install Java
We will be using open java, Get latest version from http://openjdk.java.net/install/
```sh
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update

sudo apt-get install openjdk-8-jdk
```

#### Confirm Java Version

```sh
java -version
```

#### The output should be something like this,_
```
[root@~]# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```

#### Find a java jvm path and set the java home
```sh
find /usr/lib/jvm/java-1.8* | head -n 3 
``` 
above command execution will displays a path like (```/usr/lib/jvm/java-1.8.0-openjdk-amd64```) and copy that displays path 

#### Edit Bash Profile with vim editor
```sh
vi .bashrc
```
#### edit

```sh
#JAVA_HOME=$PASTE_A_COPIED_PATH
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
PATH=$PATH:$JAVA_HOME:$HOME/bin

export PATH
```
save and exit vim editor


#### To set it permanently update your .bashrc
```sh
source ~/.bashrc
```

#### Check  java home settings by executing below command
```sh
echo $JAVA_HOME
echo $PATH
```

## Installing Jenkins
The version of Jenkins included with the default Ubuntu packages is often behind the latest available version from the project itself. In order to take advantage of the latest fixes and features, we’ll use the project-maintained packages to install Jenkins.

Following steps are used;
#### Step 1. We’ll add the repository key to the system.

```sh
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
```
When the key is added, the system will return OK. Next, we’ll append the Debian package repository address to the server’s sources.list:

#### Step 2. Add the following entry in your /etc/apt/sources.list
```sh
echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
```

#### Step 3. Now run the following commands:
```sh
sudo apt-get update

sudo apt-get install jenkins
```


### Start Jenkins

Now that Jenkins and its dependencies are in place, we’ll start the Jenkins server.

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
