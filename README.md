# Jenkins
  Running Jenkins inside a Docker container is a common practice that provides a consistent and isolated environment for Jenkins.

  ## Prerequisites
  - Ubuntu installed on VMware
  - Docker installed on Ubuntu OS

  ## Setting up Jenkins Container:
  1. Pull the Jenkins Docker Image from Docker Hub:
      ```
      docker pull jenkins/jenkins:lts
      ```
      
  2. Run Jenkins Container:
      ```
      docker run -d --name myjenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -u root jenkins/jenkins:lts
      ```
      -d: Runs the container in the background (detached mode).
      -p 8080:8080: Maps the container's port 8080 to the host's port 8080 (Jenkins web interface).
      -p 50000:50000: Maps the container's port 50000 to the host's port 50000 (Jenkins agent communication port).
      --name myjenkins: Assigns a name to the container, in this case, "myjenkins".
      -v jenkins_home:/var/jenkins_home: Saves jenkins runtime data on host machine so when container is shutdown or deleted its data and progress is saved and reusable on new container

  3. Access Jenkins:
      You can access the Jenkins web interface by opening a web browser and navigating to http://localhost:8080. The initial setup process requires obtaining the initial administrator password, which can be obtained by below command:
       
      ```
      docker exec -it myjenkins cat /var/jenkins_home/secrets/initialAdminPassword
      ```
      Copy the password and paste it into the Jenkins web interface to complete the setup. That's it! You should now have Jenkins running inside a Docker container.

  ## Getting started with Jenkins:
  1. Configure Jenkins:
      - Choose to install recommended plugins during the initial setup.
      - Create an admin user and provide necessary details.

  2. Create Your First Jenkins Job:
      - Click on "New Item" on the Jenkins dashboard.
      - Enter a name for your project, choose "Freestyle project," and click "OK."
      - Click Add Build Steps. Select Execute Shell.
      - Type:
        ```
        echo "Hello World"
        ```
      - Save and Run the Jenkins Job.  Click "Save" to save the job configuration.
      - Click "Build Now" to run the job manually.
      - Once the build is triggered, Jenkins will display the build progress and results.
      - Check the console output and any post-build actions. 
        ![Screenshot (39)](https://github.com/Agneshkastury/Jenkins/assets/154126091/5d13cc3a-a91d-4011-b19e-061ec0391eab)

  ## Building Jenkins Pipeline:
  
