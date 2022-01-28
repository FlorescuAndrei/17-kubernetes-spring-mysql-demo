
     
  Other posible way: 

Alternativ setps or other posible steps:  
    -  see box folder  
    
    
  Use NodePort instead of ingress. It provide an extern Ip, no need for ingress but the app will be accesible :  
    - apply webapp-nodeport.yaml from box folder
    
    •	minikube service webapp-service	- will allocate external Ip address and start the browser, start the browser and automatically load the app on: http://172.27.110.51:30100/customers/list  (minikube ip : nodePort) . There is no tunnel open
 
  
[BACK TO START PAGE](https://github.com/FlorescuAndrei/Start.git)

