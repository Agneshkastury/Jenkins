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
      - -d: Runs the container in the background (detached mode).
      - -p 8080:8080: Maps the container's port 8080 to the host's port 8080 (Jenkins web interface).
      - -p 50000:50000: Maps the container's port 50000 to the host's port 50000 (Jenkins agent communication port).
      - --name myjenkins: Assigns a name to the container, in this case, "myjenkins".
      - -v jenkins_home:/var/jenkins_home: Saves jenkins runtime data on host machine so when container is shutdown or deleted its data and progress is saved and reusable on new container

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
  Building a Jenkins pipeline involves creating a script in the form of a Jenkinsfile that defines the steps of your continuous integration/continuous deployment (CI/CD) process.
  We shall learn by creating pipeline for Simple Java Maven App available at Docker Hub.

  1. Create a Jenkinsfile:
     - Install Microsoft Visual Studio Code on your device.
     - Create new file named Jenkinsfile. Open the Jenkinsfile and define your pipeline.
       ```
       pipeline{
          agent any
            stages {
              stage("Clean Up"){
                 steps{
                    deleteDir()
                }
              }
              stage("Clone Repo"){
                steps{
                  sh "git clone https://github.com/jenkins-docs/simple-java-maven-app.git"
                }
              }
              stage("Build"){
                steps{
                   dir("simple-java-maven-app"){
                      sh "mvn clean install"
                   }
                }
              }
              stage("Test"){
                steps{
                  dir("simple-java-maven-app"){
                     sh "mvn test"
                  }
                }  
              }
          }
        }
       ```
      
      ![Screenshot (40)](https://github.com/Agneshkastury/Jenkins/assets/154126091/f2d562ae-68cb-41cc-b564-52430d9a6a97)

       Explanation:
        - pipeline: This keyword defines the beginning of a Jenkins pipeline.
        - agent any: Specifies that the pipeline can run on any available agent (agent is a machine where the pipeline runs).
        - stages: Defines a block containing multiple stages in the pipeline.
        - stage("Clean Up"): Defines a stage named "Clean Up."
        - steps: Contains the actions or steps to be executed within the "Clean Up" stage.
        - deleteDir(): Deletes the workspace for the current build. This is typically done at the beginning of a stage to ensure a clean environment for the subsequent steps.
        - stage("Clone Repo"): Defines a stage named "Clone Repo."
        - steps: Contains the actions or steps to be executed within the "Clone Repo" stage.
        - sh "git clone ...": Uses the sh step to execute a shell command. In this case, it clones a Git repository (simple-java-maven-app) from the specified URL.
        - stage("Build"): Defines a stage named "Build."
        - steps: Contains the actions or steps to be executed within the "Build" stage.
        - dir("simple-java-maven-app"): Changes the current directory to "simple-java-maven-app" for the following steps.
        - sh "mvn clean install": Uses the sh step to execute Maven commands. In this case, it cleans the project and installs dependencies.
        - stage("Test"): Defines a stage named "Test."
        - steps: Contains the actions or steps to be executed within the "Test" stage.
        - dir("simple-java-maven-app"): Changes the current directory to "simple-java-maven-app" for the following steps.
        - sh "mvn test": Uses the sh step to execute Maven commands. In this case, it runs the project's tests.
     
  2. Configure Jenkins to Use the Pipeline
      - Open Jenkins in your web browser at http://localhost:8080.
      - Sign-in using admin and password.
      - Click on "New Item" on the Jenkins dashboard.
      - Enter a name for your pipeline (e.g., "My Pipeline").
      - Choose "Pipeline" as the project type.
      - Scroll down to the "Pipeline" section and select "Pipeline script" in the Definition dropdown.
      - Paste your myjenkins pipeline created script in box below.
      - Save the configuration.

      ![Screenshot (43)](https://github.com/Agneshkastury/Jenkins/assets/154126091/766e80e7-3fe6-413d-8bf4-38b261060a6a)

  3. Install required packages in docker to execute maven java app.
      - Use below code:
        ```
        docker exec -it myjenkins /bin/bash
        apt-get update
        apt-get install maven -y
        ```

  3. Run the Pipeline
      - On the Jenkins dashboard, locate your pipeline project and click on it.
      - Click on "Build Now" to manually trigger the pipeline.
      - Monitor the build progress and view the console output.

     ![Screenshot (41)](https://github.com/Agneshkastury/Jenkins/assets/154126091/e99cea00-a20c-49d5-83be-5f9811cb1dd0)

     ![Screenshot (44)](https://github.com/Agneshkastury/Jenkins/assets/154126091/4c3d8fe1-a2b8-497d-81b5-4d85b5609ac5)

  4. Customize the Pipeline based on your project requirements. Explore Pipeline Visualization on the project's main page. It helps in understanding the flow of your CI/CD process.

     This pipeline performs a series of tasks: cleaning up workspace, cloning a Git repository, building the project, and running tests. Each of these tasks is organized into separate stages to provide a clear structure to the pipeline.

 
