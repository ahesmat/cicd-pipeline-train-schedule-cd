pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Deploy'){
            steps{
                withCredentials([usernamePassword(credentialsID: 'sshLogin', passwordVariable: 'passwd', usernameVariable: 'user')])
                {
                 echo "The username is $user and the password is $passwd"   
                }    
            }
    }
    }
}
