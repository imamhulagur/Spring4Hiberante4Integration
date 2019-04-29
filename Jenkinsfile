pipeline {
    agent any
  stages {
    stage ('Build') {
        
        steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '8d689245-b98e-4608-b028-74170e0fa944', url: 'https://github.com/subbu0683/Spring4Hiberante4Integration.git']]])
            sh 'mvn clean compile' 
            }
    }
     stage ('Test') {
      steps {
        echo 'Tested All the Test Cases.'
        sh 'mvn test'
        junit 'target/surefire-reports/**/*.xml' 
      }
    }
    
    stage ('SonarQube Code Analysys') {
      steps {
        echo 'SonarQube code Analysys has been checked.'
        sh 'mvn install sonar:sonar'
      }
    }
    stage ('Release') {
      steps {
        echo 'Released 1.0 Version.'
      }
    }
    stage ('Deploy / Delivery') {
      steps {
        echo 'Application has been Deployed'
        build 'Spring4Hiberante4Deployment'
      }
    }
  }
}