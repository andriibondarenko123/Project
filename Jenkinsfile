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
                            EC2_PUBLIC_IP = sh(
                                script: "terraform output ec2_public_ip",
                                returnStdout: true
                            ).trim()
                        }
                    }
                }
            }
            stage("Pull Playbook from SCM") {
                steps {
                  git 'https://github.com/andriibondarenko123/Project.git'
              }
            }
            stage("execute ansible playbook") {
                steps {
                    script {
                        echo "waiting for EC2 server to initialize" 
                        sleep(time: 90, unit: "SECONDS") 
                            
                        echo "calling ansible playbook to configure ec2 instances"
                        sh "ansible-playbook -i hosts lamp_nginx.yaml"
                    }  
                }
            }
        }   
    }
    


