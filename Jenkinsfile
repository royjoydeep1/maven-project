pipeline {
    agent any

  tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.221.77.185', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.218.247.172', description: 'Production Server')
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
                        
                        // sh "scp -i /home/ec2-user/TestDockerKey.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                        //sh "cp -i  /var/lib/jenkins/workspace/PipelineAsCodeExample/webapp/target/*.war /var/lib/tomcat8/webapps"
                        sh "sudo scp -i ~/TestDockerKey.pem /var/lib/jenkins/workspace/PipelineAsCodeExample/webapp/target/*.war /var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        
                        //sh "scp -i /home/ec2-user/TestDockerKey.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                         // sh "cp -i  /var/lib/jenkins/workspace/PipelineAsCodeExample/webapp/target/*.war /var/lib/tomcat8/webapps"
                        sh "sudo scp -i ~/TestDockerKey.pem /var/lib/jenkins/workspace/PipelineAsCodeExample/webapp/target/*.war ec2-user@${params.tomcat_prod}:/home/ec2-user/"
                        //sh "ssh -i ~/TestDockerKey.pem ec2-user@${params.tomcat_prod}"
                        sh "sudo scp -i ~/TestDockerKey.pem /home/ec2-user/*.war  /var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
