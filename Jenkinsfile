pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                echo 'Starting build'
                sh './gradlew build'
                archiveArtifacts artifacts: 'dist/trainShedule.zip'
            }
        }

    }
    
}
