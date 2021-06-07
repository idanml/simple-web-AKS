# simple-web-helm

## CLI tools
- azure CLI
- kucectl
- helm
- docker

## Main steps
1. Login to azure/AKS/ACR
- Azure login (identity)
- Azure login to AKS as admin
- integrate ACR to AKS with docker registry secret
2. Image port check
- CMD: docker pull $image
- CMD: docker history --no-trunc $image:imagetag | grep EXPOSE
3. Install ingress nginx controller
- CMD: helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
- CMD:helm repo update
- CMD: helm install $Name ingress-nginx/ingress-nginx
4. Create and deploy helm chart
- CMD: helm create $NameOfChart
- Edit requeired files
- CMD: helm upgrade --install $NameOfChart $Path

## Files
### deployment.yaml
Create the image pods with rolling strategy, cpu resource request and appropriate port.
### service.yaml
Create ClusterIP service connected with the app pods, define protocol and in/out port for access.
### ingress.yaml
Create ingress rule to access the service, define path of the app from NGINX controller.
### hpa.yaml
Create auto scalling rule depend on CPU usage.
### NOTES.txt
Provide commands to expose URL of the app.
### values.yaml
Changeable vatiables (provide by --set flag in helm upgrade command)
- app: selector of the app pods. default value: simple-web
- port: image access port. default value: 80
- deployment.replicas: num of parallel pods of the app. default value: 1
- deployment.image: app image. default value: simple-web:latest
- service.protocol: service access protocol. default value: TCP
- service.port: service access port. default value: 80
- ingress.path: URl path to app. default value: /sw
- hpa.min/maxReplicas: define the bound of auto scaling : default value: 1-10
- hpa.averageUtilization: avarage usage of CPU (by percents) to autoscalling. averageUtilization: 50

