pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    environment {
               ENV_NAME = getEnvName(env.BRANCH_NAME)
               FILE_ID = getFileIdByBranch(env.BRANCH_NAME)
     }
    stages {
        stage('Test') {
            when{
                 anyOf {
                    branch 'main'
                    branch 'release'
                 }
            }
            steps{
                echo ${ENV_NAME}
                sh "mvn test"
            }
            post{
             success {
                  junit '**/target/surefire-reports/TEST-*.xml'
                  }
            }
        }
        stage('Build') {
            when {
                  anyOf {
                                    branch 'main';
                                    branch 'release'
                                 }
            }
            steps {
                 configFileProvider(
                        [configFile(fileId: ${FILE_ID}, targetLocation: 'src/main/resources/application.properties')]) {
                        sh 'mvn  clean package -DskipTests '
                 }
            }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }

        stage('Deploy ') {
              when{
                          anyOf {
                             branch 'main'
                             branch 'release'
                          }
              }
            steps {
                sh "ls"
            }
        }

    }
}

def getEnvName(branchName) {
    if("main".equals(branchName)) {
        return "prod";
    } else if ("release".equals(branchName)) {
        return "release";
    }
}

def getFileIdByBranch(branchName) {
    if("main".equals(branchName)) {
        return "8eea3d0f-8ad1-4a2e-92cd-ad5708448769";
    } else if ("release".equals(branchName)) {
        return "8eea3d0f-8ad1-4a2e-92cd-ad5708448769";
    }
}
