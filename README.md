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
