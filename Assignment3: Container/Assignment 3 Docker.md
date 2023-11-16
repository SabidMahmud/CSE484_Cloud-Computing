# Assignment 3: Docker

Name: Sabid Mahmud
ID: 22301172

---

# 1. Installing Docker

First of all we need to install the required packages for docker. To do that, we need to run this command:

```bash
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Then, to verify docker download and install we need to add the GPG key of the installation file:

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -\
```

After that, the machine is ready to install docker. So, let's install docker using this command:

```bash
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && sudo apt install docker-ce
```

![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled.png)

Now, to check that docker has been installed successfully, run this command in the terminal.

```bash
sabid@mahmud-22301172:~$ docker --version
Docker version 24.0.7, build afdd53b
```

![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%201.png)

here, it shows that Docker version 24.0.7 has been successfully installed on my machine.

To use docker without sudo, let’s run this command:

```bash
$ sudo usermod -aG docker ${USER}
```

---

# 2. Basic Docker Commands

- `docker search` This command searches for specific images in the docker hub.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%202.png)
    
- **`docker pull`:** This command pulls a specific image from the Docker Hub. All you have to do is use the command ‘docker pull’ along with the name of the image.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%203.png)
    
- **`docker run`:** This command is used to create a container from an image. Here is how to do it:
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%204.png)
    
- `**docker ps --all`:** This command will list all the running docker containers.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%205.png)
    
- `**docker stop**`: The `docker stop` command stops a container using the container name or its id. Here is how to do it:
    
    ```bash
    $ docker stop 4e6f8c4e2c54
    ```
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%206.png)
    
- **`docker login`**: This command helps you to log into your docker hub. As you try to log in, you will be asked to give your docker hub credentials.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%207.png)
    
- **`docker commit`:** This command will create and save an image of the docker container in the local machine.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%208.png)
    
- **`docker push`:** This command helps push or upload a docker image on the repository or the docker hub.
- **`docker logs`:** This command is used to check the logs of all the docker containers with the corresponding contained id mentioned in the command. Here is how to use it:
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%209.png)
    
- **`docker rm` `docker rmi`:** These commands are used to remove docker images
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2010.png)
    
    ---
    

# 3. **Create a Docker image using Dockerfile**

At first, let’s create an empty folder where we will do the docker work. Then, Create a Dockerfile with the necessary instructions to build your Docker image. For example, let's create a simple "Hello, World!" Dockerfile that uses the NGINX web server.

Create a file named Dockerfile (with no file extension) using a text editor.

```docker
FROM nginx:latest
COPY index.html /usr/share/nginx/html
```

Now save the docker file.

Create an index.html file that we'll copy into the NGINX container. This file will display "Hello, Docker!" when someone visits the server.

Create a file named index.html:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Hello, Docker!</title>
</head>
<body>
    <h1>Hello, Docker!</h1>
    <h2>This is my very first docker project</h2>
</body>
</html>
```

![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2011.png)

**Build the Docker Image:**
Open a terminal and navigate to the directory containing your Dockerfile and index.html.

Use the docker build command to build the Docker image. The command will look like this:

![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2012.png)

Now, check if the docker image is successfully created:

![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2013.png)

We can run a container based on the newly created image using the docker run command:

```bash
$ docker run -d -p 8080:80 hello-docker
```

Access the Web Page:
Open a web browser and navigate to [http://localhost:8080](http://localhost:8080/). "Hello, Docker!" displayed on the webpage.

![Screenshot from 2023-10-31 18-53-49.png](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Screenshot_from_2023-10-31_18-53-49.png)

---

# 4. **Run a container as a single task, show outputs, and show the status of all containers (using docker ps -a)**

**Run a Container as a Single Task and Display Output:**

Run a simple container printing "Hello, Docker!" to the console:

```bash
$ docker run --name my-single-task-container ubuntu:latest echo "Hello, Docker! This is a single task container."
```

This command uses the **`ubuntu:latest`** image to run a container and executes the command **`echo "Hello, Docker!..."`** within that container. The **`--name`** flag specifies a custom name for the container as **`my-single-task-container`**.

![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2014.png)

********Let’s show the status of all the existing containers:********

```bash
$ docker ps -a
```

This command will show the status of all the containers in the device. Here we can see that the terminal automatically exited the container after the successful execution.

![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2015.png)

---

# **5. Run a container in interactive mode and install packages:**

To run a container in interactive mode and install packages, one can use the docker run command with the -it flags to start an interactive session in the container. This example demonstrates using a Ubuntu container to install packages interactively.

- **Start an Interactive Session in a Container:**
    
    Start an ubuntu container using the command
    
    ```bash
    $ docker run -it --name interactive-cont ubuntu:latest
    # the ubuntu session in docker container is open. because my terminal has changed the user name and the host name. now lets install vim, and neofetch in the docker container ubuntu.
    root@cd15084d6bb9:/# apt update && apt install vim neofetch
    ```
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2016.png)
    
    To exit the interactive mode of the container we can just enter the command `exit` inside the interactive session.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2017.png)
    
    Now, check the container status using
    
    ```bash
    $ docker ps -a
    ```
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2018.png)
    

---

# 6. Run a database container in the background. Then show logs of the running container. After, access the container in interactive mode. show some SQL queries inside the container.

- ****************************************************Run a database container:****************************************************
    
    Let’s run a Mysql container in the background.
    
    ```bash
    $ docker run -d --name database-cont -e MYSQL_ROOT_PASSWORD=secret-pw mysql:latest
    ```
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2019.png)
    
- ******************************************Show logs of running containers:******************************************
    
    Use **`docker logs`** to display the logs of the running MySQL container:
    
    ```bash
    $ docker logs database-cont
    ```
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2020.png)
    
- **Access the Container in Interactive Mode:**
    
    To enter in the interactive mode, This command connects to the MySQL server running in the container using the MySQL command-line client. It prompts for the root password set in step 1 which is `secret-pw` 
    
    ```bash
    $ docker exec -it my-mysql-container mysql -u root -p
    ```
    
    ![This is the screenshot of a database container in interactive mode.](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2021.png)
    
    This is the screenshot of a database container in interactive mode.
    
- **Execute Some SQL Queries Inside the Container:**
    
    After entering the interactive mysql shell, we can perform SQL queries. For instance, As I do not know much about mySQL, I am using the commands list databases, show tables, or create a new database:
    
    ```sql
    SHOW DATABASES;
    USE mysql;
    SHOW TABLES;
    ```
    
    ![This is an example of using the sql commands in the interactive shell.](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Screenshot_from_2023-10-31_22-09-49.png)
    
    This is an example of using the sql commands in the interactive shell.
    
- ****************************Exit the mysql shell and the container:****************************
    
    by entering the command twice we can first exit the mysql shell and then the interactive session of the container.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Screenshot_from_2023-10-31_22-11-32.png)
    

---

# **7. Push Your Image to Docker Hub:**

- ******************************************Tag the docker image:******************************************
    
    To push a docker image, it needs to be tagged in the format: `dockerhub_username/repository_name:tag`.
    
    here I am willing to push the hello docker image in the docker hub. so I ran these commands in the terminal.
    To push the image, we need to login to the docker hub account.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2022.png)
    
- ********************************************Pushing the image in the docker hub:********************************************
    
    To push the tagged image, we need to run this command:
    
    ```bash
    $ docker push sabidmahmud/hello_docker:latest
    ```
    
    ![The docker image is pushed in the docker hub.](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2023.png)
    
    The docker image is pushed in the docker hub.
    
    ![This screenshot is from my docker hub account. Here we can see the image we just pushed.](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2024.png)
    
    This screenshot is from my docker hub account. Here we can see the image we just pushed.
    

---

# **8. Set Up Your Own Private Docker Registry:**

- **Start the Private Docker Registry Container:**
    
    Run the Docker registry container using the official registry image.
    
    ```bash
    $ docker run -d -p 5000:5000 --name private-reg registry
    ```
    
    Here,
    
    - ``-**d**` runs the container in detached mode.
    - ``-**p 5000:5000**` maps the container's port 5000 to the host's port 5000.
    - **`--name private-reg`** assigns a custom name to the container.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2025.png)
    
- **Tag an Image to Push to Private Registry:**
    
    Tag the local Docker image with the address of your private registry:
    
    ```bash
    $ docker tag hello-docker localhost:5000/hello-docker
    ```
    
- **Push the Image to Your Private Registry:**
    
    Push the tagged image to the private registry:
    
    ```bash
    $ docker push localhost:5000/hello-docker
    ```
    
    This will push the image to the local registry on port 5000 with the name hello-docker.
    
    ![Screenshot from 2023-10-31 22-52-53.png](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Screenshot_from_2023-10-31_22-52-53.png)
    
    Let's check, if the docker image is pushed in the private registry successfully.
    
    We can now access the private registry by using the URL **`http://localhost:5000/v2/_catalog`** in your browser or by utilizing Docker commands.
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2026.png)
    
    - **Stop and Remove the Private Registry Container:**
        
        Once we're finished, we can stop and remove the registry container:
        
        ```bash
        $ docker stop private-reg
        $ docker rm private-reg
        ```
        

---

# 9. Create a small website or app with minimal functionality (Could be a simple HTML website that has a button that opens a static image/file) inside a Docker container. Then run the application (inside the container) in the background of your HOST machine in any port. Browse the website from your host machine.

- **********************Create and edit the html file:**********************
    
    Here I have created and edited the index.html file.
    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Hello, Docker!</title>
    </head>
    <body>
        <h1>Hello, Docker!</h1>
        <h2>This is my very first docker project</h2>
        <button onclick="openImage()"> Wellcome to Docker </button>
        <script>
            function openImage() {
                window.open('./images/whale-docker.gif', '_blank');
            }
        </script>
    </body>
    </html>
    ```
    
- ******************************************************************Dockerize the html file and the image file:******************************************************************
    
    To do this, edit the dockerfile in the project directory:
    
    ```docker
    FROM nginx:latest
    COPY index.html /usr/share/nginx/html
    COPY images/whale-docker.gif /usr/share/nginx/html/images/
    ```
    
- **Build and Run the Docker Container:**
    
    Build the Docker image and run it as a container using the following commands.
    
    ```bash
    $ docker build -t hello-docker .
    [+] Building 6.5s (8/8) FINISHED                                                                      docker:default
     => [internal] load .dockerignore                                                                               0.0s
     => => transferring context: 2B                                                                                 0.0s
     => [internal] load build definition from Dockerfile                                                            0.0s
     => => transferring dockerfile: 152B                                                                            0.0s
     => [internal] load metadata for docker.io/library/nginx:latest                                                 6.5s
     => [internal] load build context                                                                               0.0s
     => => transferring context: 102B                                                                               0.0s
     => [1/3] FROM docker.io/library/nginx:latest@sha256:add4792d930c25dd2abf2ef9ea79de578097a1c175a16ab25814332fe  0.0s
     => CACHED [2/3] COPY index.html /usr/share/nginx/html                                                          0.0s
     => CACHED [3/3] COPY images/whale-docker.gif /usr/share/nginx/html/images/                                     0.0s
     => exporting to image                                                                                          0.0s
     => => exporting layers                                                                                         0.0s
     => => writing image sha256:468ecd61b9c1b3e0225581ad2e9aa36e27ec8e0328f1af672ceb0d47d99bee7b                    0.0s
     => => naming to docker.io/library/hello-docker                                                                 0.0s
    sabid@mahmud-22301172:~/new_docker_project$ docker run -d -p 8080:80 --name hello-docker hello-docker
    816e6205daeac3aa168e57d64d9af900cc59b6a1a30c877de587cc0689d9cdf4
    ```
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2027.png)
    
- **Access the Website from Host Machine:**
    
    Open a web browser and visit **`http://localhost:8080`** to access the simple website running inside the Docker container.
    
    ![docker image is running in the website successfully.](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2028.png)
    
    docker image is running in the website successfully.
    

---

# 10. **Migrate the new container having the application into another machine. Again run the container and browse the URL.**

- ****************Export the docker image to migrate:****************
    
    Export the container as a tar file using the **`docker export`** command.
    
    ```bash
    $ docker export hello-docker > hello-docker.tar
    ```
    
    ![Untitled](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/Untitled%2029.png)
    
- ************************************************************Transfer the container into another machine:************************************************************
    
    I have send the file using google drive to my friend. He downloaded the file and run the docker image.
    In his [localhost](http://localhost), the image executed successfully.
    
    ![image.png](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/image.png)
    
    ![image.png](Assignment%203%20Docker%207294c6bc6dbc4f889ac3a63ec23cde4f/image%201.png)
    

---