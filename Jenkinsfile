pipeline {
agent any
tools { maven &#39;Maven3&#39; }
environment {
NEXUS_URL = &#39;http://localhost:8081/repository/maven-releases/&#39;
NEXUS_CREDENTIALS = credentials(&#39;nexus-creds&#39;)
}
stages {
stage(&#39;Checkout&#39;) {
steps {
git &#39;https://github.com/kevinli-webbertech/helloworld-springboot.git&#39;
}
}
stage(&#39;Build&#39;) {
steps {
bat &#39;mvn clean package&#39;
}
}
stage(&#39;Upload to Nexus&#39;) {
steps {
bat &quot;&quot;&quot;
mvn deploy:deploy-file ^
-Durl=%NEXUS_URL% ^
-DrepositoryId=nexus ^
-Dfile=target\\helloworld-0.0.1-SNAPSHOT.jar ^
-DgroupId=com.example ^
-DartifactId=helloworld ^
-Dversion=0.0.1 ^
-Dpackaging=jar ^
-DgeneratePom=true ^
-Dcredentials.username=%NEXUS_CREDENTIALS_USR% ^
-Dcredentials.password=%NEXUS_CREDENTIALS_PSW%
&quot;&quot;&quot;
}

}
}
post {
always {
archiveArtifacts artifacts: &#39;**\\target\\*.jar&#39;, fingerprint: true
}
}
}