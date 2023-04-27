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

Fallow the below path 

* Jenkins Dashboard --> Manage Jenkins --> Manage Nodes and Clouds  

![](https://github.com/kandula-uday/Jenkins/blob/main/Slave%20node/mngnodesandclouds.png)

* select new node, Give it a name, select the “permanent agent” option and click ok

![](https://github.com/kandula-uday/Jenkins/blob/main/Slave%20node/newnode.png)

![](https://github.com/kandula-uday/Jenkins/blob/main/Slave%20node/nodecreation.png)

* Fill the form give name, description 
  * **No Of Executors** --> how many concurrent builds the node can handle.
  * **Remote File System Root** --> Specify the usage directory on the node where Jenkins will store build artifacts and workspace files.
  * **Labels** --> Labels allow you to define the capabilities of a node and then assign jobs to that node based on the labels assigned.
  * **Usage** --> Use this node as much as possible.
  * **Launch Method** --> Launch Agents Vis SSH.
    * Give host address of agent
    * Select **ADD** give **Username** and **Password** of Jenkins User which was created at creation of Slave Server.

![](https://github.com/kandula-uday/Jenkins/blob/main/Slave%20node/connection.png) 

Once you click save, Jenkins will automatically connect to the slave machine and configure it as an agent.

# Setting up Jenkins slaves using ssh keys

* Login to Slava server as jenkins user

* Create an .SSH and cd into the directory.

``` mkdir ~/.ssh && cd ~/.ssh ```

* Create an ssh key pair using the following command. Press enter for all the defaults when prompted.

``` ssh-keygen -t rsa -C "The access key for Jenkins agent" ```

* Add the public to authorized_keys file using the following command.

``` cat id_rsa.pub >> ~/.ssh/authorized_keys ```

* Now, copy the contents of the private key to the clipboard.

``` cat id_rsa ```

# Add the SSH Private Key to Jenkins Credentials

* got to Jenkins Dashboard --> Manage Jenkins --> Credentials --> System --> Global Credentials 

* Paste the copied key as shown below

![](https://github.com/kandula-uday/Jenkins/blob/main/Slave%20node/ssh_jenkins.png)

