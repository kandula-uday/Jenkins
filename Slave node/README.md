# Setup Jenkins Agent/Slave Using SSH [Password & SSH Key]
One of the best features of Jenkins is its distributed nature. You can configure multiple build slaves for better segregation and scalability

# Setup Jenkins Agent/Slaves on Jenkins
There are two ways of authentication for setting up the Linux Jenkins slave agents.
1. Using username and password.
2. Using SSH Keys.

# Create a Jenkin User
It is recommended to execute all Jenkins jobs as a jenkins user on the jenkins agent nodes

#### Step 1
Create a Jenkins User

``` sudo adduser jenkins ```

Create a password for user jenkins

``` sudo passwd jenkins ```

Open the /etc/ssh/sshd_config file with a text editor

``` #PasswordAuthentication yes ```

Uncomment the line by removing the # character at the beginning of the line. Change the value to yes.

Restart the SSH service for the changes to take effect

``` systemctl restart sshd ```

# Setting Up Jenkins Slave/Agent using username and password

* Manage 

