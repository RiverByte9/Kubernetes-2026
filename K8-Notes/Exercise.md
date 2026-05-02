Exercise

The full step-by-step lab is in day1/assignment.md.

Summary of what you will do:

Install kubectl and minikube in a GitHub Codespace
Start a Minikube cluster and verify with minikube status and kubectl get nodes
Run a single nginx pod with kubectl run, inspect it, delete it — observe it stays deleted
Write a Deployment YAML for 3 nginx replicas and apply it
Delete one pod manually — watch Kubernetes recreate it (self-healing)
Scale the Deployment up to 5 and back down to 3
Break the image on purpose (nginx:doesnotexist) and debug via kubectl describe pod Events
Write a ClusterIP Service, apply it, and curl it from inside the Minikube container
Prove the Service keeps working even when you delete its backing pods
Read your ~/.kube/config file and understand what is in it
Commit all YAMLs to your repo
Deliverables: screenshots of the key steps (3 pods running, self-healing in action, image pull error, Service curl working) and your k8s/ folder pushed to GitHub.

![alt text](image.png)


![alt text](image-1.png)

![alt text](image-2.png)

![alt text](image-3.png)

![alt text](image-4.png)

![alt text](image-5.png)

![alt text](image-6.png)

![alt text](image-7.png)

![alt text](image-8.png)

![alt text](image-9.png)


![alt text](image-10.png)

![alt text](image-11.png)

![alt text](image-12.png)

![alt text](image-13.png)

![alt text](image-14.png)


**📄 What is ~/.kube/config?**

It is a configuration file used by kubectl to:

Connect to a cluster
Authenticate you
Decide which cluster you’re currently using

👉 Think of it like a connection profile

“The kubeconfig file stores cluster details, user credentials, and context mappings that allow kubectl to communicate with Kubernetes.”