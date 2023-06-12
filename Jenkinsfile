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
                    
                    sh  "scp target/vprofile-v2.war  ubuntu@35.154.203.45:~/"
                    sh  "ssh ubuntu@35.154.203.45 'sudo mv ~/vprofile-v2.war /var/lib/tomcat9/webapps/'"
                    sh  "ssh ubuntu@35.254.203.45  'sudo systemctl restart tomcat9'"
                    
                }
            }
        }

        


    }
}
