# asp.net core sample for k8s demo

1. build the image

docker build -t leixu.azurecr.io/netcore-dockerlinux:v1 -f netcore-dockerlinux/Dockerfile .
docker push leixu.azurecr.io/netcore-dockerlinux:v1

2. deploy to k8s

Create the secret (replace <acrname> <acr-pwd> and <email>)

kubectl create secret docker-registry leixuacr --docker-server=leixu.azurecr.io --docker-username=leixu --docker-password=<acr-pwd> --docker-email=<your-email>

kubectl apply -f kube-deploy.yml

3. Change the application homepage and rebuild v2

docker build -t leixu.azurecr.io/netcore-dockerlinux:v2 -f netcore-dockerlinux/Dockerfile .
leixu.azurecr.io/netcore-dockerlinux:v2

4. scale and rolling update

kubectl scale --replicas=5 ReplicationControllers/netcore-dockerlinux
kubectl rolling-update netcore-dockerlinux --image=leixu.azurecr.io/netcore-dockerlinux:v2 --update-period=10s