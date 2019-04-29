pipeline {
  agent any
  stages {
  stage ('Build Process') {
        
        steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '8d689245-b98e-4608-b028-74170e0fa944', url: 'https://github.com/subbu0683/Spring4Hiberante4Integration.git']]])
            bat label: '', script: 'mvn clean compile'
            }
    }
     stage ('Test Process') {
      steps {
        bat label: '', script: 'mvn test'
        junit 'target/surefire-reports/**/*.xml'
        echo 'JUnit Test cases are executed successfully.'
      }
    }
    
    stage ('SonarQube Code Analasys Process') {
      steps {
        bat label: '', script: 'mvn install sonar:sonar'
        echo 'SonarQube code Analysys has been checked.'
      }
    }
    stage ('Release Process') {
      steps {
        echo 'Released 1.0 Version.'
      }
    }
    stage ('Deploy / Delivery Porces') {
      steps {
        build 'Spring4Hiberante4Deployment'
        echo 'Application has been Deployed'
      }
    }
  }
}