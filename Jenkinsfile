pipeline {
    
    agent any
    
    stages {
        stage("checkout") {
            steps {
                git branch:'main', url: 'https://github.com/dtsap/course3-jenkins-gs-spring-petclinic.git'
            }
        }
        stage("build") {
            steps {
                sh "./mvnw package"    
            }
        }
        /*parallel testsA: {
            sh "echo test set A"
            sleep 2
            sleep 3
        },
        testsB: {
            sh "echo test set B"
            sleep 5
        }, testsC: {
            sleep 1
            // sh "false"
        }, failFast: true*/
        
        stage("capture") {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
                jacoco()
                junit '**/target/surefire-reports/TEST*.xml'    
            }
        }
    }
    
    post {
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
                to: 'mits@foo.bar',
                recipientProviders: [previous()],
                subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
}
