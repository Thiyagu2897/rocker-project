pipeline {
    agent any

    stages {
        stage('GIT CHECKOUT') {
            steps {
                git credentialsId: 'git-pass', url: 'https://github.com/Thiyagu2897/rocker-project.git'
            }
        }
                stage ('packaging') {
            steps {
                script{
                     sh 'npm install'
                }
            }
        }
             stage ('docker image build') {
            steps {
                script{
                     sh "docker build -t thiyagu2897/prothiyagu:v1 ."
                     sh "docker images"
                }
            }
        }
        stage('Deploy on k8s') {
            steps {
                script {
                    withKubeCredentials(kubectlCredentials: [[ credentialsId: 'kube-pass' ]]) {
                        sh 'kubectl create secret generic helm --from-file=.dockerconfigjson=/opt/docker/config.json  --type kubernetes.io/dockerconfigjson --dry-run=client -oyaml > secret.yaml'
                        sh 'kubectl apply -f secret.yaml'
                        sh 'helm package ./Helm'
                        sh 'helm install myrocket ./myrocketapp-0.1.0.tgz'
                        sh 'helm ls'
                        sh 'kubectl get pods -o wide'
                        sh 'kubectl get svc'
                    }
                }
            }
        }
    }
}
