pipeline 
{
    agent any
    

    stages 
    {
        stage('build') {
            steps {
                echo "Starting build "
                sh './gradlew build --no-daemon'
             
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
   
            stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    db = docker.build("rsv90/train-schedule")
                    db.inside {
                        sh 'echo $(curl localhost:3000)'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        //db.push("${env.BUILD_NUMBER}")
                        db.push("latest")
                    }
                }
            }
        }


        stage('DeployToProduction') 
        {
            when {
                branch 'master'
            }
            steps
            {
                //input 'Deploy to Production?'
                milestone(1)
                withCredentials([usernamePassword(credentialsId: 'web_server_cred', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) 
                {
                    script 
                        {
                        def remote = [:]
                                remote.name = 'xps'
                                remote.host = "${prod_ip}"
                                remote.user = "${USERNAME}"
                                remote.password = "${USERPASS}"
                                remote.allowAnyHosts = true
                                    sshCommand remote: remote, command: "docker pull rsv90/train-schedule:latest" 
                             try {
                                   sshCommand remote: remote, command: "docker stop train-schedule"
                                   sshCommand remote: remote, command: "docker rm train-schedule"
                                } 
                                catch (err) {
                                    echo: 'caught error: $err'
                                }
                                sshCommand remote: remote, command:"docker run --restart always --name train-schedule -p 3000:3000 -d rsv90/train-schedule:latest"
                        }
                }
            }
        }   
    }
}