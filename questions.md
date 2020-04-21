## Kub concepts questions ###



## sceduling questions ###

## We have deployed a number of PODs. They are labelled with 'tier', 'env' and 'bu'. How many PODs exist in the 'dev' environment?
kubectl get pods --selector env=dev

## how many objects labled with 'prod' environment?
kubectl get all --selector env=prod

## Identify the POD which is 'prod', part of 'finance' BU and is a 'frontend' tier?
kubectl get all --selector env=prod,bu=finance,tier=frontend

## Do any taints exist on node01?
kubectl describe node node01

## Create a taint on node01 with key of 'spray', value of 'mortein' and effect of 'NoSchedule'
kubectl taint nodes node01 spray=mortein:NoSchedule

## Do you see any taints on master node?
kubectl describe node master

## Remove the taint on master, which currently has the taint effect of NoSchedule
kubectl taint nodes master node-role.kubernetes.io/master:NoSchedule-


## Log and monitoring questions ###




## Application life-cycle questions ###




## Cluster maintenance questions ###



## Security questions ###




## Storage questions ###



## Networking questions ###




## install questions ###
