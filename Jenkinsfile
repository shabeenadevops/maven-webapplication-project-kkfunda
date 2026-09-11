//scripted-way-PL

def mavenHome = tool name: "maven3.9.16"

node {

    stage('Git Checkout') {
        git branch: 'master',
            url: 'https://github.com/shabeenadevops/maven-webapplication-project-kkfunda'
    }

    stage('COMPILE') {
        sh "${mavenHome}/bin/mvn compile"
    }

    stage('SQ REPORT') {
        sh "${mavenHome}/bin/mvn org.sonarsource.scanner.maven:sonar-maven-plugin:5.7.0.6970:sonar"
    }

    stage('Sonar Approval') {
        input message: 'SonarQube report completed. Continue?',
              ok: 'Proceed'
    }

    stage('Build') {
        sh "${mavenHome}/bin/mvn package"
    }

    stage('Deploy to Nexus') {
        sh "${mavenHome}/bin/mvn deploy"
    }

    stage('Deploy to Tomcat') {
        sh '''
            curl -u kk:password \
            --upload-file "$WORKSPACE/target/maven-web-application.war" \
            "http://3.95.250.239:8080/manager/text/deploy?path=/maven-web-application&update=true"
        '''
    }
}
