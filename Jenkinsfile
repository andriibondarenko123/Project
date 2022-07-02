pipeline {
    agent any
    stages {
        stage('provision server with Terraform') {
            environment {
                AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
                AWS_SECRET_ACCESS_KEY = credentials('jenkins_aws_secret_access_key')
            }
            steps {
                script {
                    dir('terraform') {
                    sh "terraform init"
                    sh "terraform plan"
                    sh "terraform apply --auto-approve"
                    }
                }
            }
        }
        stage("execute ansible playbook") {
            steps {
                script {
                    echo "waiting for EC2 servers to initialize" 
                    sleep(time: 90, unit: "SECONDS") 
                        
                    echo "calling ansible playbook to configure ec2 instances"
                    ansiblePlaybook credentialsId: 'newserver-ssh-key', disableHostKeyChecking: true, installation: 'ansible1', inventory: 'inventory_aws_ec2.yaml', playbook: 'lamp_nginx.yaml'
                }  
            }
        }
    }   
}
    


