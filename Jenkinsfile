pipeline {
    agent any
    stages {
        stage ('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtfacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage ('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsID: 'webserver_login', usernamevariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ],
                                transfers: [
                                    sshTransfer{
                                        sourceFiles: 'dist/trainschedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /user/bin/systemctl stop train-scheduler && rm -r /opt/train-scheduler
                                    )
                                ]  
                            )
                        ]                
                    )                    
                }
            }
       }
}
    
