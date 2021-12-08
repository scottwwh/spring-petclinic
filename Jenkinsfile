pipeline {
    agent { label 'linux' }
    stages {
        // stage ('Checkout') {
        //     steps {
        //         git 'https://github.com/scottwwh/spring-petclinic.git'
        //     }
        // }
        stage('Build') {
            agent { docker 'maven:3.8.2-jdk-11' }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
            steps {
                input 'Do you approve the deployment?'
                // echo 'Deploying...'
                sh "scp target/*.jar parallels@10.211.55.4:/opt/pet"
                sh "ssh parallels@10.211.55.4 'nohup java -jar /opt/pet/spring-petclinic-2.5.0-SNAPSHOT.jar &'"
            }
        }
    }
}