pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('Test and Build') {
        when {
        branch "master"
        }
        steps{
         configFileProvider(
                [configFile(fileId: '8eea3d0f-8ad1-4a2e-92cd-ad5708448769', targetLocation: 'src/main/resources/application.properties')]) {
                sh 'mvn  clean install'
            }
        }
          }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
         stage('Deploy') {
         when{
             branch "master"
         }
            steps {
                sh "ls"
            }
    }
}
