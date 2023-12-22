def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]

pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    parameters {
        string(name: 'GIT_BRANCH', defaultValue: 'main', description: 'Git branch to fetch code from')
        string(name: 'GIT_REPO_URL', defaultValue: 'https://github.com/example/repo.git', description: 'Git repository URL')
        string(name: 'NEXUS_URL', defaultValue: 'http://nexus.example.com:8081', description: 'Nexus Repository URL')
        string(name: 'NEXUS_REPO', defaultValue: 'your-repo-name', description: 'Nexus Repository Name')
        string(name: 'NEXUS_CREDENTIALS_ID', defaultValue: 'your-nexus-credentials-id', description: 'Nexus Credentials ID')
    }

    stages {
        stage('Fetch code') {
            steps {
                git branch: params.GIT_BRANCH, url: params.GIT_REPO_URL
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Sonar Analysis') {
            environment {
                scannerHome = tool 'sonar4.7'
            }
            steps {
                withSonarQubeEnv('sonar') {
                    sh """${scannerHome}/bin/sonar-scanner 
                        -Dsonar.projectKey=your-project-key 
                        -Dsonar.projectName=your-project-name 
                        -Dsonar.projectVersion=1.0 
                        -Dsonar.sources=src/ 
                        -Dsonar.java.binaries=target/test-classes/ 
                        -Dsonar.junit.reportsPath=target/surefire-reports/ 
                        -Dsonar.jacoco.reportsPath=target/jacoco.exec 
                        -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml"""
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage("UploadArtifact") {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: params.NEXUS_URL,
                    groupId: 'com.example',
                    version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                    repository: params.NEXUS_REPO,
                    credentialsId: params.NEXUS_CREDENTIALS_ID,
                    artifacts: [
                        [artifactId: 'your-app',
                            classifier: '',
                            file: 'target/your-app.war',
                            type: 'war']
                    ]
                )
            }
        }
    }

    post {
        always {
            echo 'Slack Notifications.'
            slackSend channel: '#jenkinscicd',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
}
