

node('master') {
    tools {
        maven 'MAVEN'
    }
    stage('checkout'){
        checkout scm
    }

    stage('Build'){
        sh 'mvn -B -DskipTests clean package'
    }



}