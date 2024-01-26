  pipeline
{
    agent any
    stages 
    {
        stage('Build') 
            {
               steps {
                    echo 'Running build automation'
                    sh './gradlew build --no-daemon'
                    sh 'echo \"test5\"'
                    archiveArtifacts artifacts: 'dist/trainSchedule.zip'
                      }
            }
         stage('Deploy')
            {
               steps {
       withCredentials([usernamePassword(credentialsId: 'webserver_login',usernameVariable: 'user', passwordVariable: 'passwd' )])
                {
                 sshPublisher(
                 continueOnError: false, failOnError: true,
                  publishers: [
                   sshPublisherDesc(
                    configName: 'staging',
                     sshCredentials: [username: "$user", encryptedPassphrase: "$passwd"],
                      
                   transfers: [
                        sshTransfer(
                         sourceFiles:'dist/trainSchedule.zip',
                         removePrefix:'/dist',
                         remoteDirectory:'/tmp',
                         execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                       )  
                      ]
                    )
                   ]
                  )
                }    
      }
     }
       
      }
 }
