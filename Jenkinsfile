pipeline {
    agent any

    environment {
        SSH_KEY = credentials('f2825fcd-e630-43da-ae1b-f0ebf49cf13e')
    }

    stages {
        stage('Build Project') {
            steps {
                bat "mvn clean install"
            }
        }

        stage('Deploy WAR File') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'f2825fcd-e630-43da-ae1b-f0ebf49cf13e', keyFileVariable: 'SSH_KEY')]) {
                    script {
                    

                        // Use scp to copy the WAR file to the remote server
                        bat "icacls  %SSH_KEY% /grant:r "ec2_user":(R)"
                        bat "ssh -o StrictHostKeyChecking=no -i C:\Users\Administrator\Downloads\rut103.pem ec2-user@54.167.127.140 'mvn -v'"
                      //  bat "ssh -i  %SSH_KEY% ec2-user@54.82.125.173 '/opt/tomcat/apache-tomcat-9.0.84/bin/startup.sh'"
                    }
                }
            }
        }

      /*  stage('Start Application') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2user1', keyFileVariable: 'SSH_KEY')]) {
                    // Use SSH to run the command to start the application
                    bat "ssh -i %SSH_KEY% ec2-user@54.82.125.173 '/opt/tomcat/apache-tomcat-9.0.84/bin/startup.sh'"
                }
            }
        }*/
    }

    post {
        success {
            echo 'Pipeline succeeded!'
            // Any cleanup or additional steps you want to perform on success
        }
        failure {
            echo 'Pipeline failed. Check the console output for details.'
            // Any cleanup or additional steps you want to perform on failure
        }
    }
}
