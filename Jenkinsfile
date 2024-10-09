pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/Karthikeyan21001828/registration-app-sanjaya.git'
        BRANCH = 'main'
        MAVEN_HOME = '/usr/share/maven' 
        TOMCAT_URL = 'http://18.206.135.232:8080/manager/text'
        TOMCAT_USER = 'admin' 
        TOMCAT_PASS = 'admin_password' 
        WAR_FILE = '/var/lib/jenkins/workspace/tomcat-job/webapp/target/webapp.war' // Path to the .war file
        APP_NAME = 'webapp' // The name of the application on Tomcat
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the GitHub repository
                git branch: "${BRANCH}", url: "${GIT_REPO}", credentialsId: 'github-token'
            }
        }

        stage('Build with Maven') {
            steps {
                // Run Maven to clean and package the project
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Deploy the .war file to Tomcat
                script {
                    sh """
                    curl --upload-file ${WAR_FILE} \
                    --user ${TOMCAT_USER}:${TOMCAT_PASS} \
                    ${TOMCAT_URL}/deploy?path=/${APP_NAME}&update=true
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
