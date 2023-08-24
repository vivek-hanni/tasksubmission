pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', credentialsId: 'vivek-hanni2demo', url: 'https://github.com/vivek-hanni/nodeapplication.git'
            }
        }
        stage('IMAGE') {
            steps {
                script {
                    def dockerTag = "1.${env.BUILD_NUMBER}" // Create a unique tag based on build number
                    bat "docker build -t hannidocker/nodeapp:$dockerTag ." // Use the generated tag
                }
            }
        }
        stage('Docker Push Hub') {
           steps {
              withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerhubpwd')]) {
                  bat 'docker login -u hannidocker -p %dockerhubpwd%'
                  script {
                      def dockerTag = "1.${env.BUILD_NUMBER}" // Reuse the same generated tag
                      bat "docker push hannidocker/nodeapp:$dockerTag"
                  }
              }
            }
			}
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'minikubeconfig')
                }
            }
        }
   }
}