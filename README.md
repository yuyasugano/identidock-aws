### References
- [Using Docker/O'Reilly](https://www.oreilly.com/library/view/using-docker/9781491915752/) 
 
### Usage
- build an identidock container
```
chmod +x entrypoint.sh
docker build -t identidock-eks .
```
- test an application with docker-compose command
```
docker-compose up -d
docker-compose stop
docker-compose rm
docker system prune
```
- push the built image to ECR [Pushing a Docker image](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)
```
docker tag <image id> <account>.dkr.ecr.<region>.amazonaws.com/identidock-aws
aws ecr get-login --region us-east-1 --profile ecr --no-include-email
docker push <account>.dkr.ecr.<region>.amazonaws.com/identidock-aws
```
- spin up a EKS cluster with the template file
```
./create_eks_cluster.sh
```
- run each replica set and service in the EKS cluster
```
kubectl config current-context
kubectl create -f redis-replicaset.yaml
kubectl create -f redis-service.yaml
kubectl create -f dnmonster-replicaset.yaml
kubectl create -f dnmonster-service.yaml
kubectl create -f identidock-replicaset.yaml
kubectl create -f identidock-service.yaml
kubectl get pods -o wide
kubectl get rs -o wide
kubectl get services -o wide
```
- delete the EKS cluster
```
eksctl delete cluster --name <cluster name>
```
 
## Troubleshooting
- container logs
```
kubectl logs <pod name>
kubectl exec -it <pod name> /bin/bash
```
 
## License

This library is licensed under the MIT License.
