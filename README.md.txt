MediaWiki Installation Using Rolling Update.

Problem Statement
We want to automate the deployment of MediaWiki using.

CFT with Any Configuration Management Tool (Only for AWS). If any -- Choose only one of your comfort. I have choosen Jenkins to deploy into minikube cluster(kubernetes)
The above automation should support CI/CD practices of chosen deployment style like Rolling Update.

Deployment Process:
To complete the deployment in a single step:
I have created one build job for Mediawiki application in Jenkins through Jenkinsfile.
where I placed Jenkins file to do the CI/CD process into minikube cluster with mediawiki.yaml file.

Finally we can browse the Mediawiki application with mediawiki services.

$ kubectl get services
````

And then access, from a web browser, to **http://[the external IP]:80**



