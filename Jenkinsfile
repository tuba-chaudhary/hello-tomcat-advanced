pipeline {
    agent any

    tools {
        jdk 'JDK21'              // Must match Global Tool Config
        maven 'Maven-3.9.11'     // Must match Global Tool Config
    }

    environment {
        WAR_FILE = "target/hello-tomcat-advanced.war"
        TOMCAT_URL = "http://localhost:8070"
        TOMCAT_USER = "tomcatuser"          // safer username, no spaces
        TOMCAT_PASSWORD = "yourpassword"   // match your tomcat-users.xml
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
                    echo "WAR file path: ${warPath}"

                    if (fileExists(warPath)) {
                        echo "Deploying WAR file to Tomcat..."
                        bat """
                            curl --upload-file "${warPath}" ^
                            --user "${TOMCAT_USER}:${TOMCAT_PASSWORD}" ^
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
