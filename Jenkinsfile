pipeline {
  agent any
  environment {
    SSH_KEY = credentials('c1aa46bd-7622-414f-8c26-c4579e245a34')
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
  }
  stages {
    stage('Build Project') {
      steps {
        sh 'mvn clean install'
      }
    } 
// transfer
        stage('Deploy to Remote Server') {
            steps {
                script {
                    def remoteServer = '44.211.82.24'
                    def remoteDirectory = '/temp/'
                    def warFileName = 'EcommerceApp.war'

                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'targetr', // The SSH configuration name from Jenkins
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: "target/${warFileName}",
                                        removePrefix: 'target',
                                        remoteDirectory: "${remoteDirectory}"
                                    )
                                ]
                            )
	@@ -36,31 +43,28 @@ pipeline {
            }
        }

//end transfer
// shutdown app

    stage('shutdown Application') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'c1aa46bd-7622-414f-8c26-c4579e245a34', keyFileVariable: 'SSH_KEY')]) {
          // Use SSH to run the command to start the application

          sh 'ssh -o StrictHostKeyChecking=no -i $SSH_KEY ec2-user@44.211.82.24 "mvn -v" '
        }
      }
    }


  }
  post {
    success {
      echo 'Pipeline succeeded!'
      // Any cleanup or additional steps you want to perform on success (t1)
    }
    failure {
      echo 'Pipeline failed. Check the console output for details.'
      // Any cleanup or additional steps you want to perform on failure
    }
   }


}
