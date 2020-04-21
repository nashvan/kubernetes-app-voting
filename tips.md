Certification Tip!
Here's a tip!

As you might have seen already, it is a bit difficult to create and edit YAML files. Especially in the CLI. During the exam, you might find it difficult to copy and paste YAML files from browser to terminal. Using the kubectl run command can help in generating a YAML template. And sometimes, you can even get away with just the kubectl run command without having to create a YAML file at all. For example, if you were asked to create a pod or deployment with specific name and image you can simply run the kubectl run command.

Use the below set of commands and try the previous practice tests again, but this time try to use the below commands instead of YAML files. Try to use these as much as you can going forward in all exercises

## Reference (Bookmark this page for exam. It will be very handy):
https://kubernetes.io/docs/reference/kubectl/conventions/


## Create an NGINX Pod

kubectl run --generator=run-pod/v1 nginx --image=nginx

# Create a new pod with the NGINX image named nging 
kubectl run nginx --image=nginx --generator=run-pod/v1

# Create a new pod with the redis image named redis in namespcase finance
kubectl run redis --image=redis --generator=run-pod/v1 -n finance

# Create a new pod with the redis:alpine image named redis and labels tier=db
kubectl run --generator=run-pod/v1 redis --image=redis:alpine -l tier=db

# which namespace has "blue" pod in it
kubectl get pods --all-namespaces


## Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml

## Generate pod template and save it in yml file
kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml > pod-template.yml


## Create a deployment

kubectl create deployment --image=nginx nginx



## Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

kubectl create deployment --image=nginx nginx --dry-run -o yaml



## Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)

kubectl create deployment --image=nginx nginx --dry-run -o yaml > nginx-deployment.yaml

## Save it to a file, make necessary changes to the file (for example, adding more replicas) and then create the deployment.


## export yaml file of current cluster and its pods, deployment, service, etc. 
kubectl get all --export=true -o yaml
# or
kubectl get po,deployment,rc,rs,ds,no,job -o yaml?

## all tips
Certification Tips - Imperative Commands with Kubectl
While you would be working mostly the declarative way - using definition files, imperative commands can help in getting one time tasks done quickly, as well as generate a definition template easily. This would help save considerable amount of time during your exams.

Before we begin, familiarize with the two options that can come in handy while working with the below commands:

--dry-run: By default as soon as the command is run, the resource will be created. If you simply want to test your command , use the --dry-run option. This will not create the resource, instead, tell you weather the resource can be created and if your command is right.

-o yaml: This will output the resource definition in YAML format on screen.



Use the above two in combination to generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch.



## POD
# Create an NGINX Pod
kubectl run --generator=run-pod/v1 nginx --image=nginx

## Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml


## Deployment
# Create a deployment
kubectl create deployment --image=nginx nginx

# Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
kubectl create deployment --image=nginx nginx --dry-run -o yaml

# Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)
kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run --replicas=4 -o yaml

The usage --generator=deployment/v1beta1 is deprecated as of Kubernetes 1.16. The recommended way is to use the kubectl create option instead.



IMPORTANT:

kubectl create deployment does not have a --replicas option. You could first create it and then scale it using the kubectl scale command.

Save it to a file - (If you need to modify or add some other details)

kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run --replicas=4 -o yaml > nginx-deployment.yaml

OR

kubectl create deployment --image=nginx nginx --dry-run -o yaml > nginx-deployment.yaml

You can then update the YAML file with the replicas or any other field before creating the deployment.

Or

kubectl create service clusterip redis --tcp=6379:6379 --dry-run -o yaml  (This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)

## Service
# Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
kubectl expose pod redis --port=6379 --name redis-service --dry-run -o yaml


Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:

kubectl expose pod nginx --port=80 --name nginx-service --dry-run -o yaml

(This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run -o yaml

(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

# how many lables, or what lables are configured on service "abc"
# what is the targetport configured on the service "abc"
kubectl describe service



Reference:

https://kubernetes.io/docs/reference/kubectl/conventions/


