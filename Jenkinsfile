pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo "Starting build by ${Person}"
                sh './gradlew build --no-daemon'
             
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
   
        stage  ('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                    script {
                    def myImage = docker.build("rsv90/trainnew:${env.BUILD_ID}")
                    myImage.inside {
                        sh 'echo $(curl localhost:3000)'
                    }
                    }
                // script {
                //     app = docker.build("rsv90/train-schedule")
                //     app.inside {
                //         sh 'echo $(curl localhost:3000)'
                //     }
                // }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
            docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login')
            {
         
               myImage.push( "${env.BUILD_ID}" )
               myImage.push("latest")
            }
            }
                // script {
                //     docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                //         app.push("${env.BUILD_NUMBER}")
                //         app.push("latest")
                //     }
                // }
            }
        }


        // stage('DeployToProduction') {
        //     when {
        //         branch 'master1'
        //     }
        //     steps {
        //         input 'Deploy to Production?'
        //         milestone(1)
        //         withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
        //             // script {
        //             //     sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker pull willbla/train-schedule:${env.BUILD_NUMBER}\""
        //             //     try {
        //             //         sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker stop train-schedule\""
        //             //         sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker rm train-schedule\""
        //             //     } catch (err) {
        //             //         echo: 'caught error: $err'
        //             //     }
        //             //     sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker run --restart always --name train-schedule -p 8080:8080 -d willbla/train-schedule:${env.BUILD_NUMBER}\""
        //             // }
        //         }
        //     }
        // }
    }
}