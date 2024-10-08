pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M2_HOME"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/vamshi4/star-agile-banking-finance.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
            
        }
        stage('Test_Reports') {
            steps {
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/bank_1', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            }
        }
        stage('created docker image') {
            steps {
                sh 'docker build -t kitt0/banking:1.0 .'
            }
        }
        stage('docker login') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'DockerLogin', usernameVariable: 'd_user', passwordVariable: 'd_password')]) {
            sh """
                docker login -u "$d_user" -p "$d_password"
            """
        }
    }
}

        stage('dockerpush') {
            steps {
                sh 'docker push kitt0/banking:1.0'
            }
        }
        
    }
}
