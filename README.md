# How to setup a EKS cluster on AWS using terrafrom?
### Install terraform on your machine
```	
    sudo yum install -y yum-utils
	sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
	sudo yum -y install terraform
	terraform --help
```		
### If you want to install on ubuntu18.04
```
	curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
	sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
	sudo apt-get update && sudo apt-get install terraform
```

### Configure aws cli
```
		aws configure
		AWS Access Key ID [None]: <___>
		AWS Secret Access Key [None]: <___>
		Default region name [None]: us-east-2
		Default output format [None]: json
```

### Clone the terraform providers aws git repo and then checkout the stable version.
```
	git clone https://github.com/hashicorp/terraform-provider-aws.git
	git checkout <VERSION>
```
```
    cd ~/terraform-provider-aws/examples/eks-getting-started
```
### Then we have to run the command ```terraform init``` which will download the plugins for providers like aws and https
```
	Installed hashicorp/http v2.0.0 (signed by HashiCorp)
	Installed hashicorp/aws v3.26.0 (signed by HashiCorp)
```		

### You have to run the command ```terraform plan``` which will going to show you what terrafrom will do. Resources that it will going to deploy on aws.
	
You can change your cluster name by making changes in ```variables.tf```
```
	variable "cluster-name" {
   default = "terraform-eks-demo"
  type    = string
  }
```

You can change the no. of worker nodes by making changes in ```eks-worker-nodes.tf```
```
    scaling_config {
    desired_size = 1
    max_size     = 1
    min_size     = 1
   }
```
						
If we want to change the region then we have to refer ```variables.tf``` file.
```	
	variable "aws_region" {
    default = "us-east-2"
    }
```						
	
After successful deployment terraform will create a ```terraform.tfstate``` file. Run ```terraform state list``` command which will give you the list of all the resources that has been deployed on aws.
```	
	""terraform output kubeconfig > ~/.kube/config""
```	

#### Then you have to install ```aws-iam-authenticator```

```
	curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.9/2020-11-02/bin/linux/amd64/aws-iam-authenticator
	chmod +x ./aws-iam-authenticator
	sudo mv aws-iam-authenticator /usr/local/bin
	aws-iam-authenticator version
```	
### Then you have to install ```kubectl```
```
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
   echo "$(<kubectl.sha256) kubectl" | sha256sum --check
   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
   mkdir -p ~/.local/bin/kubectl
   mv ./kubectl ~/.local/bin/kubectl
   kubectl version
   which kubectl
```
#### Now you are good to go, your EKS cluster will be up and running. You can check by running the below command.
```
kubectl get namespaces
kubectl get nodes -o wide
kubectl get pods -o wide
```
   

	
	
	
	
	 

	

