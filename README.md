# minikube-helm-jenkins

Verify minikube is running:
```
$ minikube status
minikube: Running
cluster: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100
```


Create namespace:
```
$ kubectl create -f minikube/jenkins-namespace.yaml
```

Create persistent volume (folder /data is persistent on minikube)
```
$ kubectl create -f minikube/jenkins-volume.yaml
```

Create service account
```
$ kubectl create -f minikube/jenkins-sa.yaml
```

Execute helm:
```
$ helm repo add jenkinsci https://charts.jenkins.io
$ helm repo update
$ helm install jenkins -f helm/jenkins-values.yaml jenkinsci/jenkins --namespace jenkins
```


Check admin password for jenkins:
```
$ printf $(kubectl get secret --namespace jenkins-project jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

Full tutorial can be found [here](https://medium.com/@lvthillo/deploy-jenkins-with-dynamic-slaves-in-minikube-8aef5404e9c1).
