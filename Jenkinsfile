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
       withCredentials([usernamePassword(credentialsId: 'web_login', passwordVariable: 'passwd', usernameVariable: 'user')])
                {
                 echo "The username is $user and the password is $passwd"   
                }    
      }
     }
       
    }
}
