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
Step 7: Go to **Launch method** and change it to **Launch agent by connecting it to master**
Step 8: Then add the current working directory path to **/home/ubuntu/jenkins** -> **Save**
Step 9: Make another node **Slave2** and **Copy Existing Node** (Slave1) -> **Save**  \
Step 10: 
