pipeline {
    agent any

    stages{
        stage('Fetch Code') 
        {
            steps {
                git branch: 'slave', url: 'git@github.com:vinnu467/vprofile-visual.git'
            }
        }

        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

        stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
    
        stage('Deploy') {
            steps {
                sshagent(credentials: ['ubuntu']) {
                    
                    sh  "scp target/vprofile-v2.war  ec2-user@13.235.104.80:~/"
                    sh  "ssh ubuntu@13.235.104.80 'sudo mv ~/vprofile-v2.war /opt/tomcat/webapps/'"
                   
                    
                }
            }
        }

        


    }
}
