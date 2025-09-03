pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'MAVEN_HOME'
    }

    environment {
        WAR_FILE = "target/hello-tomcat-advanced.war"
        TOMCAT_URL = "http://localhost:7080"
        TOMCAT_USER = "chaudhary tuba"       // replace with your Tomcat username
        TOMCAT_PASSWORD = "#tuba2102#"   // replace with your Tomcat password
    }

    stages {
        stage('Clean') {
            steps {
                bat "mvn clean"
            }
        }

        stage('Build') {
            steps {
                bat "mvn package"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def warPath = "${WORKSPACE}/${WAR_FILE}".replace("\\", "/")
                    if (fileExists(warPath)) {
                        echo "Deploying WAR file to Tomcat..."
                        bat """
                            curl --upload-file "${warPath}" ^
                            --user ${TOMCAT_USER}:${TOMCAT_PASSWORD} ^
                            "${TOMCAT_URL}/manager/text/deploy?path=/hello-tomcat-advanced&update=true"
                        """
                    } else {
                        error "WAR file not found at ${warPath}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed!"
        }
    }
}
