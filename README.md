# jenkins-ci-cd-pipeline

### Project Description
1. Three EC2 instances (Master, Slave1, Slave2)
2. Install Jenkins in Master
3. Setup a Jenkins Master-Slave cluster on AWS
4. CI/CD pipeline triggered by webhook

Step 1: Check the status of Jenkins Master  
<img src="images/Screenshot%202020-07-27%2022.43.31.png" width="550" height="100">   
Step 2: Setup admin account and login into Jenkins Dashboard
<img src="images/Screenshot%202020-07-27%2022.47.12.png" width="550" height="100">  
Step 3: Go to **Manage Jenkins** -> **Configure Global Security**  
Step 4: Change **Agents** to **Random**. Then **Save**    
<img src="images/Screenshot%202020-07-27%2022.59.36.png" width="550" height="100">  
Step 5: Now go to **Manage Nodes** -> **New Node**   
Step 6: Add **Slave1** and set as **Permanent Agent** -> **OK**  
<img src="images/Screenshot%202020-07-27%2023.19.20.png" width="550" height="100">   
Step 7: Go to **Launch method** and change it to **Launch agent by connecting it to master**  
Step 8: Then add the current working directory path to **/home/ubuntu/jenkins** -> **Save**  
<img src="images/Screenshot%202020-07-27%2023.25.07.png" width="550" height="250">  
Step 9: Make another node **Slave2** and **Copy Existing Node** (Slave1) -> **Save**  
<img src="images/Screenshot%202020-07-27%2023.26.48.png" width="550" height="200">  
Step 10: You should see the list of nodes that we have on the Jenkins Dashboard  
<img src="images/Screenshot%202020-07-27%2023.37.09.png" width="550" height="100">  
Step 11: Download **FileZilla**    
Step 12: **In Filezilla:**     
Step 13Copy the **Slave1 IP Address** as **Host**. Username: **ubuntu**. Leave the password field empty. **Port 22**    
Step 14: **Don't start the connection yet**   
Step 15: Before connecting, add your private key. Go to **Edit** -> **Settings** -> **SFTP** -> Add your key file -> **OK**  
<img src="images/Screenshot%202020-07-27%2023.47.37.png" width="800" height="100">  
Step 16: Click on **Quickconnect** --> **OK**  
Step 17: Go to Jenkins Dashboard, Click on **Slave1**. Download the **agent.jar** file by clicking on it.  (The second link down works)  
<img src="images/Screenshot%202020-07-27%2023.55.26.png" width="800" height="100">    
Step 18: Now **drag and drop** the **agent.jar** file to the ubuntu folder in FileZilla  
<img src="images/Screenshot%202020-07-27%2023.58.26.png" width="800" height="100">  
<img src="images/Screenshot%202020-07-27%2023.58.37.png" width="800" height="100">
Step 19: Let's verify if the file has been transferred to Slave1. Login to **Slave1** instance  
Step 20: Change hostname so easier to know which instance it is  
<img src="images/Screenshot%202020-07-28%2000.00.46.png" width="800" height="100">
Step 21: Now do **ls** command to verify if **agent.jar** is present  
<img src="images/Screenshot%202020-07-28%2000.01.46.png" width="800" height="100">


