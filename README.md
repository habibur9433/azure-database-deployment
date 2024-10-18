# Seting up a Database Server on an Azure VM


***In this project, I demonstrate the process of setting up a database server on an Azure Virtual Machine (VM). By configuring a database in this environment, I gain full control over the database setup, enabling extensive customization, performance tuning, and enhanced security measures. This approach is particularly beneficial for applications that require tailored database solutions, ensuring high availability and scalability. The project serves as a comprehensive guide for users interested in establishing their own database server on an Azure VM, covering everything from VM creation and database software installation to configuration steps and instructions for ensuring accessibility for authorized users.  
Pros:  
Control: Full control over the database environment and its configuration.  
Flexibility: Ability to choose and customize the database software and settings based on specific needs.  
Scalability: Easy scaling of VM resources as database demands increase.  
Security: Implementation of advanced security measures tailored to the requirements of the application.  
Cons:    
Cost: Potentially higher costs compared to managed database services.  
Maintenance: Ongoing maintenance and updates are the user's responsibility.  
Complexity: More complex setup and management compared to managed database services***  

# Step-by-Step Implementation Commands  

# Step 1: Create an Azure VM  

Log in to Azure  
`az login`  

Create a resource group  
`az group create --name rg-database --location westus`  

Create a virtual machine with username and password  
`az vm create --resource-group MyResourceGroup --name DBVM --image Ubuntu2204 --admin-username dbuser --admin-password  Password123456789@`  

![image](https://github.com/user-attachments/assets/7250cc84-76ba-4331-9120-395e1435ab99)  


# Step 2: Open Ports for Database Traffic  

Open port 3306 for MySQL traffic  

`az vm open-port --port 3306 --resource-group rg-database --name DBVM`  

![image](https://github.com/user-attachments/assets/8c9f97ca-5464-4272-82fe-19c8d8041a33)
![image](https://github.com/user-attachments/assets/7fc2594c-7cb8-450a-9368-5f4aa3998de0)  


# Step 3: Connect to the VM and Install MySQL Database  

SSH into the VM  
`ssh dbuser@<public-ip-address>`  

Update package lists  
`sudo apt-get update`  

Install MySQL server  
`sudo apt-get install mysql-server -y`  

Secure MySQL installation  
`sudo mysql_secure_installation`  
Proceed through the prompts to create the root password and secure the installation.  

![image](https://github.com/user-attachments/assets/a919558c-0da2-429b-b8c8-550d66450ef3)  
![image](https://github.com/user-attachments/assets/9d6032a1-305c-4e9f-87cc-e334f0183e01)  
![image](https://github.com/user-attachments/assets/cf0cd3b5-053b-42c5-8bf1-a99ea7236ed5)
![image](https://github.com/user-attachments/assets/df09d3ce-b110-48bf-82b0-dfa9edc0395d)




# Step 4: Configure MySQL for Remote Access  

Edit MySQL configuration file  
`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`  

Change the bind-address from 127.0.0.1 to 0.0.0.0 to allow remote access  
`bind-address = 0.0.0.0`  
type 'i' to edit and ':wr' to save and exit  

Restart MySQL service to apply changes  
`sudo systemctl restart mysql`

![image](https://github.com/user-attachments/assets/5f926c11-71f1-4ac6-a0b7-40cbcae63f9c)

![image](https://github.com/user-attachments/assets/938448a6-925e-47df-a215-7ffd2c0633d8)



# Step 5: Create a Database and User  

Log in to MySQL as root  
`sudo mysql -u root -p`  
then enter the root password     

Create a new database  
`CREATE DATABASE azure_db_project;`  

Create a new user and grant privileges  

`CREATE USER 'dbuser1'@'%' IDENTIFIED BY 'Password123@';`  
`GRANT ALL PRIVILEGES ON azure_db_project.* TO 'dbuser1'@'%';`  
`FLUSH PRIVILEGES;`  
`EXIT;`  

![image](https://github.com/user-attachments/assets/d17111e4-370b-418d-8485-9db2a34cee56)  

 

# Step 6: Verify new user login  

login as the new user created in previous step    

`sudo mysql -u dbuser1 -p`  

enter the new user password   

![image](https://github.com/user-attachments/assets/2e710abb-9939-4cf2-a55f-a0bb2db0d2ba)  



***Azure portal overview***  

![image](https://github.com/user-attachments/assets/5495b94c-85d0-48ec-b21a-0fbf83e9a708)


***Clean up the resources to aviod additional costs***  

`az group delete --name rg-database --yes --no-wait`  

![image](https://github.com/user-attachments/assets/7e258b23-ddb8-4091-add0-aa6111f359d5)







