# Graduation-project

## Deploying backend application on kubernetes cluster using CI/CD jenkins pipeline

- **build infrastructure using terraform and apply it to GCP**
    
    `terraform init` 
    
    `terraform plan`
    
    `terraform apply` 
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled.png)
    

- **confirm that kubernetes cluster build successfully**
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%201.png)
    

- **connect vm via ssh and install kubernetes & docker**
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%202.png)
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%203.png)
    

- **authenticate to GKE via vm and gcloud auth  and show current namespaces**
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%204.png)
    

### **Build jenkins image from Dokerfile**

       

![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%205.png)

- build image
    
    `docker build . -f jenkins_with_docker -t jenkins-final`
    
- tag the image
    
    `docker tag jenkins-final ahmedabdelsalam19/jenkins-final`
    
- push image to DockerHub
    
    `docker push ahmedabdelsalam19/jenkins-final`
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%206.png)
    

### Build application image from Dockerfile

- clone app from github repo
    
    `git clone [https://github.com/mahmoud254/jenkins_nodejs_example.git](https://github.com/mahmoud254/jenkins_nodejs_example.git)`
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%207.png)
    

- build app image from Dockerfile
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%208.png)
    
- build the image
    
    `docker build . -f dockerfile -t node-app`
    
- tag the image
    
    `docker tag node-app ahmedabdelsalam19/node-app`
    
- push the image to DockerHub
    
    `docker push ahmedabdelsalam19/node-app`
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%209.png)
    

### Deployment jenkins on GKE

- copy k8s to vm  using secure copy
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2010.png)
    
- create jn-ns namespace
    
    `kubectl create namespace jn-ns`
    
- make jn-ns default
    
    `kubectl config set-context --current --namespace=jn-ns`
    
- create SA for jenkins
    
    `kubectl apply -f jn-SA.yml`
    
- create volume  [pvc]
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2011.png)
    
- create deployment for jenkins
    
    `kubectl apply -f jn-dep.yml`
    
- create jenkins service with LoadBalancer type
    
    `kubectl apply -f jn-svc.yml`
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2012.png)
    
- you can access jenkins through → [http://34.79.231.32:8080/](http://34.79.231.32:8080/)
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2013.png)
    

- add credentials to jenkins for [`my GitHub - my DockerHub - management vm`]
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2014.png)
    

- Build a CI/CD Jenkins Pipeline to Deploy a nodeapp
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2015.png)
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2016.png)
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2017.png)
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2018.png)
    
- you can access url → [http://35.205.211.72:3000/](http://35.205.211.72:3000/)
    
    ![Untitled](Graduation-project%20b2a4375d36e64dffbf09115999ebb5e4/Untitled%2019.png)