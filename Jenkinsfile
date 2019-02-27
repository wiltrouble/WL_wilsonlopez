pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh './task-10/gradlew clean build -p task-10'                   
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh './task-10/gradlew test -p task-10'
                sh './task-10/gradlew jacocoTestReport -p task-10'   

                // Publish the JUnit test Report
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'task-10/build/reports/tests/test',
                    reportFiles: 'index.html',
                    reportName: 'JUnit Report'
                  ]

                // Publish the Java Code Coverage Report
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'task-10/build/jacocoHtml',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Report'
                  ]                              
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh './task-10/gradlew jar -p task-10'                
            }
        }
    }

    post {
        always {            
            archiveArtifacts artifacts: 'task-10/build/libs/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: 'task-10/build/libs/**/*.war', fingerprint: true
            junit 'task-10/build/test-results/**/*.xml'
        }
    }
}
