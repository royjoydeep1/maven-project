pipeline {
    agent any

  tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'ec2-18-221-209-192.us-east-2.compute.amazonaws.com', description: 'Staging Server')
         //string(name: 'tomcat_prod', defaultValue: '18.219.98.154', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -r -i /home/ec2-user/TestDockerKey.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

               /* stage ("Deploy to Production"){
                    steps {
                        sh "scp -r -i /home/ec2-user/TestDockerKey.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }*/
            }
        }
    }
}
