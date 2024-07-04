pipeline {
    agent any
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 1, unit: 'SECONDS')
    }
    
    stages {

        stage('regular release') {
            when {
                anyOf {
                    branch "release/*"
                }
                not {
                    anyOf {
                        branch "release/*-beta*"
                    }
                }
            }
            steps {
                echo 'Regular release'
            }
        }

        stage('beta release') {
            when {
                anyOf {
                    branch "release/*-beta*"
                }
            }
            steps {
                echo 'beta release'
            }
        }
    }
}