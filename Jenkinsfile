def isVersionUpdated = 0
pipeline {

    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '15'))
    }

    parameters {
        booleanParam(name: 'DISABLE_VERSION_BUMP_ONLY_CHECK', defaultValue: false, description: '')
        booleanParam(name: 'FORCE_PUBLISH', defaultValue: false, description: '')
    }


    stages {
        stage('Version change') {
            steps {

                        isVersionUpdated = 0
            }
        }


        stage('Publish') {
            steps {
                script {

                    if (params.FORCE_PUBLISH == true) {
                        isVersionUpdated = 1
                    }
                    if (isVersionUpdated > 0 ) {
                        println "EXECUTED"
                    }
                }
            }
        }
    }
}
