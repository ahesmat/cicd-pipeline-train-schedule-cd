 pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                sh 'echo \"test5\"'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
     stage('Deploy'){
      when{
       branch 'master'
      }
      steps{
       withCredentials([usernamePassword(credentialsId: 'web_login', passwordVariable: 'passwd', usernameVariable: 'user')])
                {
                 sshPublisher(
                 continueOnError: false, failOnError: true,
                  publishers: [
                     sshCredentials: [(username: "${user}", encryptedPassphrase: "{$passwd}")],
                      configName: "staging"
                   transfers: [
                        sshTransfer(
                         sourceFiles:"dist/trainSchedule.zip"
                         removePrefix:"/dist"
                         remoteDirectory:"/tmp"
                         execCommand: "sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule"
                       )  
                      ]
                   ]
                  )
                }    
      }
     }
       
    }
}
