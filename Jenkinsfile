pipeline {
    agent {
        label 'master'
    }
    options { 
      skipDefaultCheckout() 
    }
    stages {
        stage('SCM Checkout'){
            steps {
                script {
                    echo "SCM Checkout Begins"
                    def mvnHome
                    mvnHome =  tool name: 'apache-maven-3.6.0', type: 'maven'
                    git 'https://github.com/subbu0683/Spring4Hiberante4Integration.git'
                        if (isUnix()) {
                         	sh "'${mvnHome}/bin/mvn' clean package -DskipTests docker:build"
                      	} else {
                            bat label: 'Build', script: "\"${mvnHome}\"\\bin\\mvn -Dmaven.test.failure.ignore clean package" 
                      	}
                    echo "SCM Checkout Ends ${mvnHome}"
                }
            }
        }
        
        stage('Compilation') {
            steps{
                script{
                    echo "Compilation Begins"
                    def mvnHome
                    mvnHome =  tool name: 'apache-maven-3.6.0', type: 'maven'
                    if (isUnix()) {
                        echo 'Build process in Unix'
                        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
                    } else {
                        echo 'Build process Windows'
                        bat label: 'Build', script: "\"${mvnHome}\"\\bin\\mvn -Dmaven.test.failure.ignore clean package"
                    }
                    echo "Compilation Ends"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps{
                script{
                    echo "SonarQube Analysis Begins"
                    def mvnHome
                    mvnHome =  tool name: 'apache-maven-3.6.0', type: 'maven'
                        echo '${env.BRANCH_NAME} Begins'
                                withSonarQubeEnv('SonarQube') {
                                    if (isUnix()) {
                                        echo 'SonarQube Analysis in Unix'
                                        sh "${mvnHome}/bin/mvn sonar:sonar"
                                        } 
                                    else {
                                       echo 'SonarQube Analysis in Windows'
                                       bat label: 'SonarQube Analysis', script: "\"${mvnHome}\"\\bin\\mvn sonar:sonar"
                                    }
                                }
                        echo '${env.BRANCH_NAME} Ends'
                   
                    echo "SonarQube Analysis Ends"
                }
            }
        }
        
        stage("SonarQube Quality Gate Status Check"){
            steps{
                script{
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                            if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
        
   }
        
    post {
        always {
            echo 'JENKINS PIPELINE'
        }
        success {
            echo 'JENKINS PIPELINE SUCCESSFUL'
        }
        aborted {
            echo 'JENKINS PIPELINE ABORTED'
        }
        failure {
            echo 'JENKINS PIPELINE FAILED'
        }
    }
}
