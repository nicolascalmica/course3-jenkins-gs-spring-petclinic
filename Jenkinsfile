// Small change
echo "hello world"

// This block indicates a declarative pipeline
pipeline {
    agent any
    
    stages {
        // We don't need this stage because we are using the default one
        // stage("checkout") {
        //     steps {
        //         git branch: 'main', url: 'https://github.com/nicolascalmica/course3-jenkins-gs-spring-petclinic.git' 
        //     }
        // }
        
        stage("build") {
            steps{
                bat "mvn package"
            }
        }
        
        stage("capture"){
            steps{
                archiveArtifacts artifacts: '**\\target\\*.jar'
                jacoco()
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
    }
    
    // In this way, this step will be performed any time the build is finished
    // even if the result is failed 
    post {
        always{
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}", 
                recipientProviders: [developers()], 
                subject: "${env.JOB_NAME}(${BUILD_NUMBER}) is ${currentBuild.currentResult}",
                to: 'nicolas@calmi.it'
        }
    }    
}