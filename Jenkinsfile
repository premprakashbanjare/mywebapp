node {
    def app
    stage('Clone Repository') {
        checkout scm
    }

    stage('Build Image') {
        app = docker.build("471836749170.dkr.ecr.us-east-1.amazonaws.com/app/mywebapp")
    }
    stage('Testing Docker Image') {
        app.inside {
            sh 'echo "Test passed"'
        }
    }
    stage('Push Image') {
        docker.withRegistry('471836749170.dkr.ecr.us-east-1.amazonaws.com/app/mywebapp') {
            app.push("$env.BUILD_NUMBER")
            app.push ("latest") 
        }
    }
    stage('Kubernetes Deployment') {
        withKubeConfig([credentialsId:'kbnt-cred',serverUrl: 'https://15D6022A00899E60CE1CDD289B47A2AC.gr7.us-east-1.eks.amazonaws.com']) {
            sh ("kubectl --kubeconfig=/home/kube/config apply -f pod.yaml")
        }
    }
}