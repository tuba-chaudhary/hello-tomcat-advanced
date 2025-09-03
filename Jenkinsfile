pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven-3.9.11'
    }

    environment {
        WAR_FILE = 'target/hello-tomcat-advanced.war'
        TOMCAT_URL = 'http://localhost:7080'
        TOMCAT_USER = 'chaudhary tuba'
        TOMCAT_PASSWORD = '#tuba2102#'
    }

    stages {
        stage('Clean Project') {
            steps {
                bat "mvn clean"
            }
        }

        stage('Build Project') {
            steps {
                bat "mvn package"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFilePath = "${WORKSPACE}/${WAR_FILE}".replace("\\", "/")
                    echo "WAR file path: ${warFilePath}"

                    if (fileExists(warFilePath)) {
                        echo 'WAR file found, deploying to Tomcat...'
                        bat """
                            curl --upload-file "${warFilePath}" \
                            --user "${TOMCAT_USER}:${TOMCAT_PASSWORD}" \
                            "${TOMCAT_URL}/manager/text/deploy?path=/hello-tomcat-advanced&update=true"
                        """
                    } else {
                        error('WAR file not found! Build failed.')
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Build completed.'
        }
    }
}
pipeline {
    agent any

    tools {
        jdk 'JDK21'        // Use exact name from Jenkins Global Tool Config
        maven 'Maven-3.9.11'
    }

    environment {
        WAR_FILE = "target/hello-tomcat-advanced.war"
        TOMCAT_URL = "http://localhost:7080"
        TOMCAT_USER = "chaudhary tuba"       // update with your Tomcat username
        TOMCAT_PASSWORD = "#tuba2102#"   // update with your Tomcat password
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
