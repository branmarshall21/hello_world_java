node('master') {

     stage('checkout'){
         checkout scm
     }

     stage('Build'){

         git url: https://github.com/branmarshall21/hello_world_java.git
         withMaven(
          // Maven installation declared in the Jenkins "Global Tool Configuration"
           maven: 'M3',
           // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
           // Maven settings and global settings can also be defined in Jenkins Global Tools Configuration
           mavenSettingsConfig: 'my-maven-settings',
           mavenLocalRepo: '.repository') {
           // Run the maven build
            sh "mvn clean install"
         }
     }
 }