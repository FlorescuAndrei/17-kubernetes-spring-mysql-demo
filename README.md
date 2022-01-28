# 17-kubernetes-spring-mysql-demo
Kubernetes learning project.    
   
Take a previous app,  [14-thymeleaf-demo](https://github.com/FlorescuAndrei/14-thymeleaf-demo.git),  build a docker image.    
Upload the image to docker hub, see: [16-docker-spring-boot-mysql-demo](https://github.com/FlorescuAndrei/16-docker-spring-boot-mysql-demo.git).   

Install Minikube and kubectl.   
Create YAML files :  
  -  namespace.yaml,
  -  secret.yaml,
  -  mysqldb.yaml.  
  -  webapp.yaml  
  -  webapp-ingress.yaml  
    
    
Start minikube and use kubectl to apply files.
After that, app is running and can be acces on local DNS webapp.com  


Steps:  
 1. Start minikube  
    -  minikube start --driver hyperv  
    Additional commands that may be useful:  
      -  minikube start --memory=4098 --driver hyperv        &emsp; &emsp; &emsp;          #if you need more memory (default 2200) 
      -  minikube delete        &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;        #destroy all files in minikube 
  
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
     -  kubectl get pod --watch -n customer-namespace	        &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;&emsp; &emsp; &emsp;	 #the console wait for changes
     -  kubectl describe pod mysqldb-deployment-64f9d4988f-9khxw -n customer-namespace             &emsp; &emsp; &emsp;      #pod name will be different.
     -  kubectl logs mysqldb-deployment-64f9d4988f-9khxw -n customer-namespace
 5. Create app deployment and service  
     -  kubectl apply -f webapp.yaml
     -  kubectl get pod -n customer-namespace
     -  kubectl logs webapp-deployment-656d6bb7d7-wpj9d -n customer-namespace
   
 6. Create ingress for accessing app in a friendly user way: webapp.com
     -  minikube addons enable ingress	          &emsp; &emsp; &emsp;    &emsp; &emsp; &emsp;    #install ingress controller
     -  kubectl get ns 
     -  kubectl get pods -n ingress-nginx                  
     -  kubectl apply -f webapp-ingress.yaml
     -  kubectl get ingress -n customer-namespace
     -  kubectl describe ingress -n customer-namespace webapp-ingress
 
 7. Resolve local DNS name 'webapp.com' to minikube ip    
     -  minikube ip 
      
 If you are running Minikube locally, use minikube ip to get the external IP. The IP address displayed within the ingress list will be the internal IP.     
 
 Add this line to the bottom of the c:\windows\system32\drivers\etc\hosts file on your windows computer (172.17.72.152 = minikube ip)  
 
      -  172.17.72.152  webapp.com   
 
    

 
  Tips  
  - very important to allocate more ram when crating minikube. I run in a lot of problem without knowing why . Even with pods working, when I run webapp.com in browser  I could not access the page because everything was runing very slowly in minikube.
  - kubectl get pod    &emsp; --> &emsp;    status must be running, If not wait a few minutes
  - check logs
  - minikube addons enable ingress      &emsp; --> &emsp;  if return error problably minikube runs out of memory, repet commad.
  - kubectl get pods -n ingress-nginx  &emsp; --> &emsp;   ingress-nginx-controller must be running   
  - to edit /etc/hosts file run notepad as administrator and then open file.
  - in app application.properties the url to database contains the database service name (mysqlc1): 
      - spring.datasource.url=jdbc:mysql://**mysqlc1**/web_customer_tracker 
         
         
Alternativ setps or other posible steps:  
    -  see [box](https://github.com/FlorescuAndrei/17-kubernetes-spring-mysql-demo/tree/main/box) folder           
         
     
  
 
  
  
[BACK TO START PAGE](https://github.com/FlorescuAndrei/Start.git)

