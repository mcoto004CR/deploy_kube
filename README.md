# How to deploy an image in Kubernetes - Raspberry Pi

## Deploy an image in Kubernetes

I recommend you look at my other two repositories first
  [Deploy Kubernetes on raspberry pi](https://github.com/mcoto004CR/deploy_kube)
  [Create a custom image](https://github.com/mcoto004CR/nginx-custom-image)


## Deploy an nginx image
###  Simple CLI command
    
    kubectl create deployment nginx --image=nginx
    
### Using YAML file
    
    #nginx.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx-deployment
    spec:
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx
 
 Deploy using this command: 
 
      kubectl apply -f nginx.yaml
      
      To check depplyment:
      
          kubectl get deployments

## Deploy Node Port Service

### Thru CLI
  
    kubectl create service nodeport nginx --tcp=80:32000
    
## Using YAML

    #nginxservice.yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: nginx-service
    spec:
      type: NodePort
      selector:
        app: nginx
      ports:
        - protocol: TCP
          port: 80
          nodePort: 32000
          
  Then run: 
  
      kubectl apply -f nginxservice.yaml
  
  Validate the service deployment
  
      kubectl get services
      You should see the service deployed on port 32000
      
  Test the deployment
  
    curl [IP address of any node (RPi)]:[Port by the Node Port]
    ie curl 192.168.42.101:32000
    You should see the HTML of the nginx page
    
    
  If you are getting a error that cannot get the page, see below
  
  
## IMPORTANT iptables issues on Pi

If you have issue with reaching between PODs run this command after boot

    sudo iptables -P FORWARD ACCEPT
    https://github.com/coreos/flannel/issues/799

If that didn't work after reboot, modify this file on all nodes

    Edit /etc/sysctl.conf
    Uncomment or add the following line: net.ipv4.ip_forward=1
    Reboot
    
