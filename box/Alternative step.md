
     
  Other posible way: 

  Use NodePort instead of ingress. 
  It provide an extern Ip.
  At step 5:  
    - kubectl apply webapp-nodeport.yaml &nbsp; &nbsp; &nbsp; &nbsp;  # instead of webapp.yaml    
    -  minikube service webapp-service	&nbsp; &nbsp; &nbsp; &nbsp; # will allocate external Ip address and start the browser,  and automatically load the app on: http://172.27.110.51:30100/customers/list  (minikube ip : nodePort).  
      
       
       
  Notes:  
  We should use externalized property files for apps.
  I changed application.properties to application.yaml and app works. It is still internal file, i must find a way to externalized it.
  
  
  
[Back to 17-kubernetes-spring-mysql-demo.git](https://github.com/FlorescuAndrei/17-kubernetes-spring-mysql-demo.git) 

