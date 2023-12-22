pipeline{
    agent{label 'master'}
    tools{maven 'M3'}
    stages{
        stage('Compilation'){
            steps{
                sh 'mvn compile' 
            }
        }
        stage('Build'){
            steps{
                sh 'mvn test' 
            }
        }
        stage('Package'){
            steps{
                sh 'mvn package' 
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    // One or more steps need to be included within each condition's block.
                }
                success{
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        stage('Deploy'){
            steps{
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']){
                sh 'nohup java -jar -Dserver.port-8001 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &' 
            }
            }
        }
    }
}
