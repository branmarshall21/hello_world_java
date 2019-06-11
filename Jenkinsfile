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
                      withSonarQubeEnv('SonarDev') {
                           sh './gradlew sonarqube -Dsonar.host.url=http://sonar.steadystatecd.com -Dsonar.login=$SONAR_TOKEN'

                        }
                  }

            }

         stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }


        stage('Build Docker') {
            when {
                branch 'master'
            }
            steps {
                sh 'docker build . -t 550522744793.dkr.ecr.us-east-1.amazonaws.com/userapi:${BUILD_NUMBER}'
                sh 'docker tag 550522744793.dkr.ecr.us-east-1.amazonaws.com/userapi:${BUILD_NUMBER} 550522744793.dkr.ecr.us-east-1.amazonaws.com/userapi:latest'
                sh '/home/jenkins/ecr-login.sh | /bin/bash '
                sh 'docker push 550522744793.dkr.ecr.us-east-1.amazonaws.com/userapi:${BUILD_NUMBER}'
                sh 'docker push 550522744793.dkr.ecr.us-east-1.amazonaws.com/userapi:latest'
            }
        }

        stage('Dev Deploy') {

                when {
                branch 'master'
            }

            steps {
                 sh 'ecs-deploy -c dev-APICluster -n userapi -i 550522744793.dkr.ecr.us-east-1.amazonaws.com/userapi:${BUILD_NUMBER} -r us-east-1 --timeout 420 '

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