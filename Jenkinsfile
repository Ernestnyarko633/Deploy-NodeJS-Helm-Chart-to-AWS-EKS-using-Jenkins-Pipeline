pipeline {
  agent any
    
  tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_credential', url: 'https://github.com/Ernestnyarko633/Deploy-NodeJS-Helm-Chart-to-AWS-EKS-using-Jenkins-Pipeline/']]])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t henrynarko/node-app-1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'henrynyarko', variable: 'henrynyarko')]) {
                    sh 'docker login -u henrynyarko -p ${henrynyarko}'
                 }  
                 sh 'docker push henrynyarko/node-app-1.0'
                }
            }
        }
         
     stage('Deploying Node App to helm chart on eks') {
      steps {
        script {
            sh ( 'aws eks update-kubeconfig --name sample --region us-west-2')
            sh "kubectl get ns"
            sh "helm install nodeapp ./node-app"
          kubernetesDeploy(configs: "nodeapp-deployment-service.yml", kubeconfigId: "kubernetes_config")
        }
      }
    }

  }
}
