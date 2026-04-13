pipeline {
    agent any

    tools { 
        maven 'Maven3' 
    }

    environment {
    NEXUS_URL = 'http://host.docker.internal:8081/repository/maven-releases/'
    NEXUS_CREDENTIALS = credentials('nexus-creds')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mruthiks1/nexus_hw6.git',
                    credentialsId: 'github-creds'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh """
                mvn deploy:deploy-file \
                -Durl=$NEXUS_URL \
                -DrepositoryId=nexus \
                -Dfile=target/helloworld-0.0.1-SNAPSHOT.jar \
                -DgroupId=com.example \
                -DartifactId=helloworld \
                -Dversion=0.0.1 \
                -Dpackaging=jar \
                -DgeneratePom=true \
                -Dusername=$NEXUS_CREDENTIALS_USR \
                -Dpassword=$NEXUS_CREDENTIALS_PSW
                """
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }
    }
}
