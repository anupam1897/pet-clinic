pipeline {
    agent any
    tools {
        jdk 'jdk11'
        maven 'maven3'
        
    }
    environment {
        EMAIL_RECIPIENTS = 'anupam1897@gmail.com'
    }
    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/anupam1897/pet-clinic.git'
            }
        }
        stage('Build Step') {
            steps {
                sh 'mvn clean package'
            }
        }
        // stage('Sonar Analysis') {
        //     steps {
        //         sh ''' mvn sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=squ_9a34b867d06e2effadd5bfcbb778370568c30d6b -Dsonar.projectName=Petclinic -Dsonar.java.binaries=. -Dsonar.projectKey=Petclinic '''
        //     }
        // }
    }
    
    post{
        always{
            script {
                def buildResult = currentBuild.currentResult
                def buildNumber = env.BUILD_NUMBER
                def projectName = env.JOB_NAME
                def buildUrl = env.BUILD_URL
                def buildDuration = currentBuild.durationString

                def emailBody = """
                    <p>Build Result: ${buildResult}</p>
                    <p>Project Name: ${projectName}</p>
                    <p>Build Number: ${buildNumber}</p>
                    <p>Build URL: ${buildUrl}</p>
                    <p>Build Duration: ${buildDuration}</p>
                """

                emailext(
                    subject: "Build #${buildNumber} - ${buildResult}",
                    body: emailBody,
                    recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'DevelopersRecipientProvider']],
                    to: "${env.EMAIL_RECIPIENTS}"
                )
            }
        }
    }
}
