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
                echo 'Build'
            }
        }

        stage('Dev Deploy') {

                when {
                branch 'master'
            }

            steps {
                echo 'dev deploy when master'
            }

        }

        stage('DEV Test') {
            when {
                branch 'master'
            }
            steps {

                parallel(
                        "Integration Test ": {
                            echo 'Run integration tests'
                        },
                         "Functional Test ": {
                            echo 'Run integration tests'
                        }

                )
            }
        }

         stage('QA deploy') {
             when {
                branch 'master'
            }

            steps {
                echo 'Deploy QA'
                echo 'Sanity Checks'
            }

        }

        stage('QA Functional Tests') {
            when {
                branch 'master'
            }
            steps {
                echo 'Unit Test'
            }
        }


         stage('Performance TEST') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploy QA'
                echo 'Sanity Checks'
            }

        }


        stage('Deploy Stage') {
           when {
                branch 'master'
            }
            steps {
                echo 'deploy stage'
                echo ' Stage Sanity Checks'
            }
        }
    }
    post {
        always {
             publishHTML (target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build/reports/checkstyle',
                    reportFiles: '*.html',
                    reportName: "CheckStyle Report"
                    ])
               publishHTML (target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build/reports/findbugs',
                    reportFiles: '*.html',
                    reportName: "Findbugs Report"
                    ])
                   publishHTML (target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build/reports/pmd',
                    reportFiles: '*.html',
                    reportName: "PMD Report"
                    ])

                        publishHTML (target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'build/reports',
                    reportFiles: 'dependency-check-report.html',
                    reportName: "Dependency Check Report"
                    ])

        }

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