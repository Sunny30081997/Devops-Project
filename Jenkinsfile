pipeline {
  agent any
  
  stages {
    stage('Build Project') {
      steps {
        sh 'mvn clean install'
      }
    } 
    // Transfer
    stage('Deploy to Remote Server') {
      steps {
        script {
          def remoteServer = 'localhost'
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
            ]
          )
        }
      }
    }

    
       stage('Deploy WAR File') {
      steps {
                  script {

           
          sh 'ssh ec2-user@localhost "bash /home/ec2-user/commamd.txt"'
           // sh 'ssh -o StrictHostKeyChecking=no -i $SSH_KEY ec2-user@54.158.93.160 "mvn -v"'
         
          
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
