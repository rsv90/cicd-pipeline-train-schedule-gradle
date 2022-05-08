pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                echo 'Starting build'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: './dist/trainShedule.zip'
            }
        }

    }
    
}
