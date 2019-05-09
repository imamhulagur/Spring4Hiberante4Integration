node {
   def mvnHome
   stage('Preparation') { // for display purposes
      git 'https://github.com/subbu0683/Spring4Hiberante4Integration.git'
      mvnHome =  tool name: 'apache-maven-3.6.0', type: 'maven'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         echo 'Build process in Unix'
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         echo 'Build process Windows'
        bat label: 'Build', script: "\"${mvnHome}\"\\bin\\mvn -Dmaven.test.failure.ignore clean package"
      }
   }
  stage('SonarQube Analysis') {
         withSonarQubeEnv() { 
         if (isUnix()) {
            echo 'SonarQube Analysis in Unix'
            sh "${mvnHome}/bin/mvn sonar:sonar"
            } 
         else {
           echo 'SonarQube Analysis in Windows'
           bat label: 'SonarQube Analysis', script: "\"${mvnHome}\"\\bin\\mvn sonar:sonar"
          }
        }
   }
   stage("Quality Gate Statuc Check"){
            timeout(time: 1, unit: 'HOURS') {  
            def qGate = waitForQualityGate()
            echo '${qGate.status}'
            if (qGate.status != 'OK') {
                error "Pipeline aborted due to quality gate failure: ${qGate.status}"
            }else if (qGate.status == 'OK'){
                 echo 'SonarQube Analysis status is OK'
            }else  if (qGate.status == 'IN_PROGRESS'){
                 echo 'SonarQube Analysis status is IN_PROGRESS'
            }
        }
   }
}
