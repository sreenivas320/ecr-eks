pipeline {
    agent any
    parameters {       
        password(name: 'access_key', defaultValue: '', description: 'Enter a access key')
        password(name: 'secret_key', defaultValue: '', description: 'Enter a secret key')
		choice(name: 'action', choices: ['destroy', 'apply', 'plan'], description: 'provide the terraform action')
		string(name: 'cluster_name', defaultValue: '', description: 'cluster_name')
		string(name: 'cluster_version ', defaultValue: '', description: 'cluster_version ')
		string(name: 'subnet_cidr_blocks', defaultValue: '', description: 'subnet_cidr_blocks')
		string(name: 'instance_types', defaultValue: '', description: 'instance_types')
		string(name: 'desired_size', defaultValue: '', description: 'desired_size')
		string(name: 'max_size', defaultValue: '', description: 'max_size')
		string(name: 'min_size', defaultValue: '', description: 'min_size')
		
    }
    stages{
        stage('Checkout SCM'){
            steps{
                script{
                    git branch: 'sree', credentialsId: 'github_cred', url: 'https://github.com/sreenivas320/ecr-eks.git'
                }
            }
        }
    stage('Initializing Terraform'){
            steps{
                script{
                    dir('ecr-eks'){
                         sh 'terraform init'
                    }
                }
            }
        }    
	stage('Validating Terraform'){
            steps{
                script{
                    dir('ecr-eks'){
                         sh 'terraform validate'
                    }
                }
            }
        }
        stage('Previewing the infrastructure'){
            steps{
                script{
                    dir('ecr-eks'){
                         sh 'terraform plan -var access_key=$access_key  -var secret_key=$secret_key -var cluster_name=$cluster_name -var cluster_version=$cluster_version -var subnet_cidr_blocks=$subnet_cidr_blocks -var vpc_cidr_block=$vpc_cidr_block -var instance_types=$instance_types -var desired_size=$desired_size -var max_size=$max_size -var min_size=$min_size'
                    }
                    input(message: "Approve?", ok: "proceed")
                }
            }
        }
        stage('Create/Destroy an EKS cluster'){
            steps{
                script{
                    dir('ecr-eks'){
                         sh 'terraform $action --auto-approve -var access_key=$access_key  -var secret_key=$secret_key -var cluster_name=$cluster_name -var cluster_version=$cluster_version -var subnet_cidr_blocks=$subnet_cidr_blocks -var vpc_cidr_block=$vpc_cidr_block -var instance_types=$instance_types -var desired_size=$desired_size -var max_size=$max_size -var min_size=$min_size'
                    }
                }
            }
        }	
    }
}




        
