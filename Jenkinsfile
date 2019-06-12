@Library('jenkins_pipeline_library')_

pipeline {
    agent any

    options {
        skipDefaultCheckout()
        timeout(time: 10, unit: 'MINUTES')
        timestamps()

    }



    stages {



      stage('Checkout') {
            steps {
                checkout scm

            }
      }

        stage('Build') {
            steps {
                echo 'Build'
            }
        }

        stage('Unit Test') {
            steps {
                 parallel(
                        "UnitTest ": {
                            echo 'Run Units tests'
                        }

                )
            }
        }

        stage('Code Analysis') {
            steps {

                parallel(
                        "Lint ": {
                            echo 'Lint'
                        },
                        "PMD ": {
                            echo 'PMD'
                        },
                         "CheckStyle ": {
                              echo 'Checkstyle'
                        },
                         "FindBugs ": {
                             echo "Find Bugs"
                        },

                        "OWASP Check": {
                            echo "OWASP Check"
                        }

                )
            }
        }


        stage('Sonar') {

                  steps {
                      echo 'Sonar Analysis'
                  }

            }




        stage('Build Docker') {
            when {
                branch 'master'
            }
            steps {
                echo 'Build Docker image'
            }
        }


    }
    post {
        echo 'post pipeline status'

        failure {
            echo 'Failed'
        }

        success {
            echo 'Success!'


        }

        unstable {
            echo 'Unstable'
        }

    }
}