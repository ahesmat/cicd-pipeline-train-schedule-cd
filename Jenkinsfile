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
         stage('Print branch name'){
           steps{
           script {
                    echo "Current Branch Name: ${env.BRANCH_NAME}"
                }
           }
         }
         stage('Deploy To Staging')
            {
               when {
                   branch 'master'
                 }
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
      stage('Deploy To Production')
            {
               when {
                   branch 'master'
                 }
               steps {
                 script
                 {
                 input(message:"Good to go?",ok:"yes")
                   milestone(1)
                 }
                 
       withCredentials([usernamePassword(credentialsId: 'webserver_login',usernameVariable: 'user', passwordVariable: 'passwd' )])
                {
                 sshPublisher(
                 continueOnError: false, failOnError: true,
                  publishers: [
                   sshPublisherDesc(
                    configName: 'production',
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
