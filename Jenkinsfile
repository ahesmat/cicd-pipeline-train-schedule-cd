 pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                sh 'echo \"test3\"'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
     stage('Deploy'){
      steps{
       withCredentials([usernamePassword(credentialsId: 'sshLogin', passwordVariable: 'passwd', usernameVariable: 'user')])
                {
                 echo "Hello"   
                }    
      }
     }
       
    }
}
