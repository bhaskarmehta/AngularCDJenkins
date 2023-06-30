pipeline{
    agent any
    stages{
        stage('clone repo'){
            steps{
              checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/bhaskarmehta/AngularCDJenkins.git']]])  
            }
        }
        stage('Integrate Jenkins with EKS Cluster and Deploy App') {
            steps {
                withAWS(credentials: 'aws_credentials', region: 'us-east-1') {
                  script { 
                    sh "curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip"
                    sh "apt install unzip"  
                    sh "unzip awscliv2.zip"
                    sh "./aws/install"
                    sh "curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.11/2023-03-17/bin/linux/amd64/kubectl"
                    sh "chmod +x ./kubectl"
                    sh "mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin"
                    sh ('aws eks update-kubeconfig --name mycluster --region us-east-1')
                    sh "kubectl apply -f Deployments/"
                    }
               }
            }
        }
    }
}