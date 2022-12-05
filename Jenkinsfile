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
                    branch 'main';
                    branch 'release'
                 }
            }
            steps{
                sh "mvn test"
            }
        }
        stage('Build prod') {
            when {
                  anyOf {
                                    branch 'main';
                                    branch 'release'
                                 }
            }
            steps {
                 configFileProvider(
                        [configFile(fileId: '8eea3d0f-8ad1-4a2e-92cd-ad5708448769', targetLocation: 'src/main/resources/application.properties')]) {
                        sh 'mvn  clean package -DskipTests '
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

        stage('Deploy ${ENV_NAME}') {
             when{
                 branch "main"
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
