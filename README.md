# Deployment of Simple Notes App
This is a simple notes app built with React and Django. This is a fork repository that needs to be deployed using Nginx and Jenkins.

## Requirements
1. Python 3.9
2. Node.js
3. React

## Deployment Stages

### First Stage
1. First, create an instance using AWS and choose Ubuntu image. Start the instance and connect it to your local terminal using ssh like this 
```
ssh -i {pem file} @ubuntu{ IP address of the machine }
```

2. Now update the environment
```
sudo apt-get update
```

Install two major things: Nginx and docker
```
sudo apt install nginx
```

```
sudo apt install docker.io
```

3. Now enable Nginx
```
sudo systemctl start nginx
```

4. Now if you copy the ip address given by the instance in AWS and paste it into a web browser you'll see something like this:

![image](https://cdn.wp.nginx.com/wp-content/uploads/2014/01/welcome-screen-e1450116630667.png)


### Second Stage

1. Clone the repository
```
git clone https://github.com/LondheShubham153/django-notes-app.git
```

2. Build the app
```
docker build -t notes-app .
```

3. Run the app
```
docker run -d -p 8000:8000 notes-app:latest
```

4. Open nginx file using this
```
vim /etc/nginx/sites-enabled/default
```

5. In vim editor add these lines
![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/24efabfd-5445-40aa-bc1a-4392fe073092)

This is known as Reverse Proxy.

6. There are still some issues will persist about frontend rendering, so go to build directory like this:

```
cd /djago-notes-app/mynotes/build
```

Now copy the whole file using this command and paste into /var/www/html:
```
sudo cp -r * /var/www/html
```

Now open your web browser and type the ip address of your instance you'll find something like this:

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/b7e7dd18-6070-4755-a1af-ec247f53a538)

### Third Stage
1. In this stage we'll use Jenkins. To download and install jenkins you can use their official documentation [link](https://www.jenkins.io/doc/book/installing/linux/)

2. Since Jenkins work on port 8080, so go to your instance, click security, then click on security groups. Over there, click "Edit Inbound Rules" and add the port 8080

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/d6ef67dc-ad95-4e4d-b84c-f2eaad52e18b)

3. After that open your web page type ip address of your instance with port number 8080 you'll see the Jenkins page. Create an account with username and password.

4. After successful account creation go to **Manage Jenkins** -> **System**. In **GitHub** add a credential like Personal access tokens (classic) from **GitHub Settings**.

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/f9d4c197-a052-45ef-b49d-368e5480b7e6)

5. Next click **Create a Job**

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/f11b4515-37f8-4649-9c70-f41cffe35461)

6. Create a freestyle project

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/7fcbef25-2355-48b0-9eff-b21facc11441)

7. Add the git repository

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/4c8a8764-c3a6-4485-8d32-2f9ab2113905)

8. Add Build Steps and save 

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/e1a4263e-d8d1-42ca-be6e-48c489e9df1f)


9. Now go to **dashboard** -> **notes-app** and press **Build Now**

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/1d1189d8-a0b7-4202-9d60-3fdd25bdf684)

If it builds successfully you'll see this:

![image](https://github.com/Argha-Majumder/django-notes-app-cicd/assets/81928385/30130a39-603a-4232-b821-cfacab0c3d1c)
