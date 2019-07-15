# Remote Debug Tomcat EC2
The objective is to connect a Eclipse Java Debugger to a remote tomcat running on AWS EC2 Instance (Amazon Linux AMI).



## Environment Information
 * Tomcat: tomcat 9.0.x (through yum)
 * Instance type: EC2 t2.micro
 * OS: Amazon Linux 2 AMI
 
 

## Software already installed
 * Tomcat 9.x.
 
 ## Configuration Steps
 
 **1. Find the environment file for Tomcat**
 
 * If you installed tomcat9 through yum command this path '/etc/tomcat9/tomcat9.conf' is correct. 
```
whereis tomcat9
vim /etc/tomcat9/tomcat9.conf
```

* If you installed the tomcat by yourself you should create the setenv.sh file if you have not set the environment file (setenv.sh) yet. In your tomcat "folder/bin" create a sentenv.sh file and add the execution rights.
```
cd /opt/tomcat9/bin
touch setenv.sh
chmod +x setenv.sh
```


**2. Set the JPDA Configuration**

In the tomcat9.conf or setenv.sh file you need to add the following configuration to the JPDA_OPTS variable. In my configuration I have chosen the 8000 for my debug port.

* For the tomcat9.conf file:
```
JPDA_OPTS="-agentlib:jdwp=transport=dt_socket,address=8000,server=y,suspend=n $JPDA_OPTS"
```

* For the setenv.sh file:
```
JPDA_OPTS="-agentlib:jdwp=transport=dt_socket,address=8000,server=y,suspend=n $JPDA_OPTS"
```

**3. Restart Tomcat**
```
service tomcat9 restart
```


**4. Create a SSH-tunnel to the debug port**

By creating a ssh tunnel you won’t need to add any security group to the ec2 instance so you will be able to skip the firewall. In your local machine create a ssh tunnel to the debug port using the following command:

* Mac
```
ssh -N -v -L 8000:127.0.0.1:8000 -L 8000:127.0.0.1:8000 ec2-user@ec2xxxxx.compute.amazonaws.com -i <your aws key.pem>
```

* Windows

**Note:** If you are in Windows you can set up the tunnel using Putty with the following configuration:

![putty](https://github.com/Yogendra0Sharma/RemoteDebugEC2Tomcat/blob/master/Tunnels_Config.PNG)
![putty](https://github.com/Yogendra0Sharma/RemoteDebugEC2Tomcat/blob/master/Tunnels_Config1.PNG)
![putty](https://github.com/Yogendra0Sharma/RemoteDebugEC2Tomcat/blob/master/AuthKey.PNG)
![putty](https://github.com/Yogendra0Sharma/RemoteDebugEC2Tomcat/blob/master/AuthKey.PNG)
![putty](https://github.com/Yogendra0Sharma/RemoteDebugEC2Tomcat/blob/master/connect.PNG)

**6. Launch the Eclipse debug**
![Eclipse](https://github.com/Yogendra0Sharma/RemoteDebugEC2Tomcat/blob/master/eclipse%20debug.PNG)

_**Any improvement or comment about this info is always welcome! As well as others shared their code publicly I want to share mine! Thanks!**_
