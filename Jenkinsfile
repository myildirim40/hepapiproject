pipeline {
  agent any
  
  stages {
    stage('Terraform Apply') {
      steps {
        script {
          sh '''
            terraform init
            terraform apply -auto-approve
          '''
        }
      }
    }
    
    stage('SSH to Instance') {
      steps {
        sshagent(['Firstkey']) {
          script {
            def ip = sh(returnStdout: true, script: 'terraform output -raw instance_public_ip')
            sh "ssh -o StrictHostKeyChecking=no -i /path/to/private/key ubuntu@${ip} 'git clone https://github.com/myildirim40/mvc-flask-pymongo.git & cd mvc-flask-pymongo/k8s/ & kubectl apply -f .'"
          }
        }
      }
    }
  }

 post {
       
        failure {

            echo 'Deleting Terraform Stack due to the Failure'
                sh 'terraform destroy --auto-approve'
        }
    }


}
