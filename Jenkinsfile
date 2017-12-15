pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.91.211.129', description: 'Production Server')
         string(name: 'tomcat_prod', defaultValue: '52.205.250.130', description: 'Staging Server')
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
                        bat "scp -i "C:\Users\\mik\\Downloads\\timcat-demo.pem" **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i "C:\\Users\\mik\\Downloads\\timcat-demo.pem" **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
