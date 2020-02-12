Using Terraform, deploy two Kubernetes clusters in two major Cloud providers across two different global regions.
Use Global Load Balancing to route traffic between the two clusters
Deploy a stateful containerized application across both clusters
Automate the deployment of the application using tools of your choice
Integrate the deployment pipeline operations into a chat-ops tool
Perform blue/green deployment of the application across both cluster

---
Decisions:

AWS/EKS in us-east-1
Azure/AKS in canada central
Application will be a custom flask application

---
Prerequisite: AWS account and Azure account

0. Install tools

a. terraform
```
$ https://www.terraform.io/downloads.html
$ mv terraform /usr/local/bin
$ cd arctiq-ext-mission-greg-reboul
$ terraform init
```

b. helm v2 (only version compatible with federations)
```
brew install helm@2
brew link --force helm@2
helm init --service-account tiller
```







6. Installing a federation

7. The application

```
pipenv install flask

```

8. Create domain name (subdomain)
```
global.earlyfrench.ca
```
-> Route53





10. setting up a git repository
git remote add  github https://github.com/gregov/arctiq-ext-mission.git



TODO:

0. Install an ingress

1. Refine the github action to leverage the releases

2. Use Route53 to perform a global load balancing

3. Automate deployment and set up a chat-ops 


Feedback:
	Mike: use a more distant region to showcase lag -> done, GCP in northern europe
	Kyle: use GCP -> done

---
Work log

Monday Feb 10th
5:30 received the challenge
6:00 installed terraform and iterm2
7:00 configured terraform with aws and azure
9:00 Azure AKS is up and running
9:30 AWS EKS is up and running
9:45 Destroying the configs and calling it a day

Tuesday Feb 11th
6:30am Created a new AWS account to leverage free tier
10:30am Federation is installed
12:00pm First github action
5:00pm Federation has all clusters connected
7:00pm Helm deploy seems to be working

Wednesday Feb 12th
9:00am Federating GCP + AWS + Azure

Problems:
Instances not available in Azure Canada East
-> move to CA-central
I changed the instance to the smallest possible -> Hit the quota
-> back to DS1_v2
AWS desynchronized with Terraform, the roles are not destroyed
-> had to manually destroy resources using the console
Destroying takes forever
-> Azure kubectl doesn't merge properly
kubefed-charts/kubefed is not compatible with Helm3
-> pinned to v2
tiller pod is taking forever to launch 
-> I had a single node
kubefed unistall is buggy
-> thre is an additional manual step to take
github actions docker push fails
-> you need to have a personal token setup (and it's not documented)
helm pullimage is not working properly
-> Actually my image is way to big / connection too slow
the federation cannot join for some reasons
-> kubefedctl requires a very specially formatted kube config
secrets are not working on remote cluster to access github registry

the federation did not survive a replacement of nodegroups on the host

Questions:
How to create a persistant storage cross-cloud ?



References:
https://learn.hashicorp.com/terraform/aws/eks-intro
https://docs.microsoft.com/en-us/azure/terraform/terraform-create-k8s-cluster-with-tf-and-aks
https://aws.amazon.com/blogs/startups/from-zero-to-eks-with-terraform-and-helm/
https://medium.com/google-cloud/planet-scale-microservices-with-cluster-federation-and-global-load-balancing-on-kubernetes-and-cd182f981653
https://github.com/terraform-providers/terraform-provider-aws.git
https://github.blog/2019-07-29-whats-new-in-the-github-and-slack-integration/
https://github.com/kubernetes-sigs/kubefed/blob/master/docs/userguide.md
https://www.nickaws.net/aws/elixir/2019/09/02/Federation-and-EKS.html
https://www.katacoda.com/openshift/courses/introduction/federated-clusters
https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/
