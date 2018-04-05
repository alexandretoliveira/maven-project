pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '54.165.154.88', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.165.154.88', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        sh "sudo cp **/target/*.war /var/lib/docker/volumes/tomcat_webapp/_data/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "sudo cp **/target/*.war /var/lib/docker/volumes/tomcatprod_webapp_prod/_data/"
                    }
                }
            }
        }
    }
}