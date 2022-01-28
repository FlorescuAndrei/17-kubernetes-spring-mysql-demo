# 17-kubernetes-spring-mysql-demo
Kubernetes learning project.    
   
Take a previous app,  [14-thymeleaf-demo](https://github.com/FlorescuAndrei/14-thymeleaf-demo.git),  build a docker image.    
Upload the image to docker hub, see: [16-docker-spring-boot-mysql-demo](https://github.com/FlorescuAndrei/16-docker-spring-boot-mysql-demo.git).   

Install Minikube and kubectl
Create YAML files :  
  -  namespace.yaml,
  -  secret.yaml,
  -  mysqldb.yaml.  
  -  webapp.yaml  
  -  webapp-ingress.yaml  
    
    
Start minikube and use kubectl to apply files.
After that, app is running and can be acces on local webapp.com  


Steps:  
 1. Start minikube  
    -  minikube start --driver hyperv
    Additional commands that may be useful:  
      -  minikube start --memory=4098 --driver hyperv &nbsp&nbsp&nbsp #if you need more memory (default 2200)
      -  minikube delete  &nbsp&nbsp&nbsp #destroy all files in minikube 
  
  2. Create namespace for the app. Optional step 
     -  kubectl get ns
     -  kubectl apply -f customer-namespace.yaml
    
 3. Create secret.yaml   
     -  kubectl apply -f mysql-secret.yaml
     -  kubectl get secret -n customer-namespace
    
 4. Create database deployment and service 
     -  kubectl apply -f mysqldb.yaml
     -  kubectl get all -n customer-namespace
     -  kubectl get pod -n customer-namespace
     -  kubectl get pod --watch -n customer-namespace		#the console wait for changes
     -  kubectl describe pod mysqldb-deployment-64f9d4988f-9khxw -n customer-namespace         #pod name will be different.
     -  kubectl logs mysqldb-deployment-64f9d4988f-9khxw -n customer-namespace
 5. Create app deployment and service  
     -  kubectl apply -f webapp.yaml
   
 6. Create ingress for accessing app in a friendly user way: webapp.com
     -  minikube addons enable ingress		#install ingress controller
     -  kubectl get ns 
     -  kubectl get pods -n ingress-nginx
     -  kubectl apply -f webapp-ingress.yaml

 
 
  
     
     
  Alternativ setps or other posible steps: 
1. modify database original mysql image to match app specification:  add new user and password, add database schema:
    - pull original mysql image, run in container with changes, commit new image.(creating table with data will not be saved since there are no volume set)
      - docker run --name mysqldb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=web_customer_tracker -e MYSQL_USER=student -e MYSQL_PASSWORD=student -d mysql
    - create new image from container:
      - docker commit mysqldb  florescua/16-mysql-demo:0.1 
    - create a new repository:
      - florescua/16-mysql-demo
    - push new image to docker hub: 
      - docker push florescua/16-mysql-demo:0.1
      
2. create a network to run the app, no docker compose and .yaml file:  
  - docker network create net1
  - docker run --name mysqlc1 -p 3306:3306  -d  florescua/16-mysql-demo:0.1
  - docker network connect net1 mysqlc1
  - docker run -d -p 8080:8080 --name 16-customer-demo-app --network net1 florescua/16-customer-demo-app:0.1
  - docker network inspect net1
  - docker network ls
  - docker inspect 16-customer-demo-app
 

Notes  
Error building the .jar file:
  - After modify application.properti and change localhost to mysqlc1, the app will not run on local environment.
Will generate build error and no .jar file will be created. To build it, delete all target file and rebuild the app 
Eclipse project –run as- maven build – goals = package; **check skip test**  
  
[BACK TO START PAGE](https://github.com/FlorescuAndrei/Start.git)

