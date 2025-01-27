pipeline {
    agent any
    parameters {
        choice(name: 'BUILD_TOOL', choices: ['Maven', 'Gradle'], description: 'Choose build tool')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
    }
    environment {
        MAVEN_HOME = tool name: 'MavenTool' // Replace 'Maven' with the configured Maven tool name in Jenkins
        GRADLE_HOME = tool name: 'GradleTool' // Replace 'Gradle' with the configured Gradle tool name in Jenkins
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/smathankar/jenkins-app.git'
            }
        }
        stage('Build Application') {
            steps {
                script {
                    if (params.BUILD_TOOL == 'Maven') {
                        echo 'Building with Maven...'
                        sh "${env.MAVEN_HOME}/bin/mvn clean install"
                    } else if (params.BUILD_TOOL == 'Gradle') {
                        echo 'Building with Gradle...'
                        sh "${env.GRADLE_HOME}/bin/gradle build"
                    }
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    if (params.BUILD_TOOL == 'Maven') {
                        echo 'Running Maven tests...'
                        sh "${env.MAVEN_HOME}/bin/mvn test"
                    } else if (params.BUILD_TOOL == 'Gradle') {
                        echo 'Running Gradle tests...'
                        sh "${env.GRADLE_HOME}/bin/gradle test"
                    }
                }
            }
        }
        stage('Archive Artifacts') {
            steps {
                script {
                    if (params.BUILD_TOOL == 'Maven') {
                        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                    } else if (params.BUILD_TOOL == 'Gradle') {
                        archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
