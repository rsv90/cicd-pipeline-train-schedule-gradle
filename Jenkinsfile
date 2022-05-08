pipeline {
    agent any

    stages {
        stage('build') {
            input{
                message 'Should we proceed?'
                ok "Yes, go ahead"
                submitter 'rsv'
                parameters {
                    string(name:'Person', defaultValue:'Nobody', description: 'type a name')
                }
            }
            steps {
                echo 'Starting build by ${Person}'
                sh './gradlew build --no-daemon'
             
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }

    }
    
}
