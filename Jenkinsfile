def isVersionUpdated = 0
pipeline {

    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '15'))
    }

    environment {
        IS_VERSION_BUMPED=0
    }

    parameters {
        booleanParam(name: 'DISABLE_VERSION_BUMP_ONLY_CHECK', defaultValue: false, description: '')
        booleanParam(name: 'FORCE_PUBLISH', defaultValue: false, description: '')
    }


    stages {
        stage('Publish') {
            steps {
                script {

                    if (params.FORCE_PUBLISH == true) {
                        env.IS_VERSION_BUMPED = 1
                    }
                    if (env.IS_VERSION_BUMPED > 0 ) {
                        println "EXECUTED"
                    } else {
                        println "not executed"
                    }
                }
            }
        }
    }
}
