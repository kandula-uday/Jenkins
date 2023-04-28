# Setup Jenkins Agent/Slave Using SSH [Password,SSH Key & Agent.Jar] 
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

# Setting up jenkins slaves using Agent.jar File

1. Provide below information to add Jenkins agent  
   **Name:** uniquely identifies an agent within this Jenkins installation  
   **Description:** <Description>  
   **Number of executors:** 2  
   **Remote root directory:** /home/jenkins/jenkins-agent   
   **Labels:** Labels (or tags) are used to group multiple agents into one logical group.  
   **Usage:**  
   - Use this node as much as possible  
   - Only build jobs with label expressions matching this node   
 
   **Launch method:**   
   	  - Launch agent by connecting it to the master   
   	  - Launch agent via execution of command on the controller  

   **Custom WorkDir path:**  custom Remoting work directory will be used instead of the Agent Root Directory  
   **- Use WebSocket [x]**  
   **Availability:**  
   	- Keep this agent online as much as possible  
   	- Bring this agent online according to a schedule  
   	- Bring this agent online when in demand, and take offline when idle 

2. Once you save above configuration you will get a command which should be executed in the agent. it contains agent.jar, a secret-file, and a jnlp file 

3. Copy the jar file to the jenkins slave server. execute the below commands. this will setup the connection with master node.

```sh
     echo "secret_key" > secret-file
     java -jar agent.jar -jnlpUrl http://<Jenkins_URL>/computer/abc/jenkins-agent.jnlp -secret @secret-file -workDir "/home/jenkins/jenkins-slave"
   ```

4. Once connected you can create or edit a job to chose this option in the `Restrict where this project can be run`

![](https://github.com/kandula-uday/Jenkins/blob/main/Slave%20node/agentjatcommand.png)

![](https://github.com/kandula-uday/Jenkins/blob/main/Slave%20node/Screenshot%202023-04-27%20at%202.09.13%20PM.png)

