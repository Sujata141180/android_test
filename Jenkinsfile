pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        echo 'Hello from checkout'
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/sujata8073/Java-webapp.git']]])
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Archeive') {
      steps {
        archiveArtifacts 'target/my-webapp.war'
      }
    }
    stage('Nexus publisher') {
      steps {
        nexusPublisher(nexusInstanceId: '12345', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/my-webapp.war']], mavenCoordinate: [artifactId: 'my-webapp', groupId: 'com.mycompany.app', packaging: 'war', version: '3.6']]])
      }
    }
    stage('Deploy') {
      steps {
        sh 'rm -rf ./*.war'
        sh 'wget http://34.73.131.70:8081/repository/maven-releases/com/mycompany/app/my-webapp/3.6/my-webapp-3.6.war'
        sh 'sudo cp my-webapp-3.6.war /var/lib/tomcat8/webapps/my-webapp-3.6.war'
      }
    }
    stage('Blueocean') {
      steps {
        echo 'Hello from blueocean'
      }
    }
  }
}