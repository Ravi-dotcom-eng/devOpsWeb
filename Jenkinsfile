pipeline {
    agent any
    
    tools {
        maven 'MAVEN'
    }
    parameters {
         string(name: 'staging_server', defaultValue: '35.92.109.243', description: 'Remote Staging Server')
    }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        deploy adapters: [tomcat9(credentialsId: 'tom', path: '', url: 'http://35.92.109.243:8080/')], contextPath: null, war: '**/*.war'
                    }
                }
            }
        }
    }
}
