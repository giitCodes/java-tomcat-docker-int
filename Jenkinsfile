pipeline {
    agent any
    stages {
        stage('Build Application') {
            steps {
                sh 'mvn -f pom.xml clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts...."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Create Tomcat Docker Image'){
            steps {
                sh "pwd"
                sh "ls -a"
                sh "docker build . -t tomcatsamplewebapp:${env.BUILD_ID}"
                sh "docker tag tomcatsamplewebapp giitcodes/tomcatsamplewebapp:${env.BUILD_ID}"
            }
        }

        stage('Login to Docker'){
            steps {
                withDockerRegistry([credentialsId: "dockerhub", url: ""]) {
                sh "docker push giitcodes/tomcatsamplewebapp:${env.BUILD_ID}"
                }
            }
        }
    }

    post {
        always {
            sh "docker logout"
        }
    }
}
