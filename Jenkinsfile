pipeline {
    agent any
  stages {
    stage ('Build Compile') {
        
        steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '8d689245-b98e-4608-b028-74170e0fa944', url: 'https://github.com/subbu0683/Spring4Hiberante4Integration.git']]])
            sh 'mvn clean compile' 
            }
    }
     stage ('Testing ') {
      steps {
        echo 'Testing.'
        sh 'mvn test'
        junit 'target/surefire-reports/**/*.xml' 
      }
    }
      stage ('SonarQube Code Analysys ') {
      steps {
        echo 'SonarQube.'
        sh 'mvn install sonar:sonar'
      }
    }
    stage ('Deploy') {
      steps {
        echo 'Deploy.'
      }
    }
  }
}