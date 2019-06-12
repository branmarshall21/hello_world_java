node('master') {

    stage('checkout'){
        checkout scm
    }

    stage('Build'){
        sh 'mvn -B -DskipTests clean package'
    }



}