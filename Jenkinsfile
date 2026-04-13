pipeline {
    agent any

    tools { 
        maven 'Maven3' 
    }

    environment {
        NEXUS_URL = 'http://host.docker.internal:8081/repository/maven-releases/'
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

        stage('Deploy to Nexus') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-creds',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {

                    sh '''
cat > settings.xml <<EOF
<settings>
  <servers>
    <server>
      <id>nexus</id>
      <username>${NEXUS_USER}</username>
      <password>${NEXUS_PASS}</password>
    </server>
  </servers>
</settings>
EOF

mvn deploy:deploy-file \
  -Durl=$NEXUS_URL \
  -DrepositoryId=nexus \
  -Dfile=target/helloworld-0.0.1-SNAPSHOT.jar \
  -DgroupId=com.example \
  -DartifactId=helloworld \
  -Dversion=0.0.1 \
  -Dpackaging=jar \
  -DgeneratePom=true \
  --settings settings.xml
'''
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }
    }
}
