def isVersionUpdated = 0
pipeline {

    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '15'))
    }

    parameters {
        choice(name: 'deploymentTarget', choices: ['PRE', 'TEST', 'PP', 'PROD' ], description: 'Env for deploy?')
    }


    stages {

        stage('Deploy PRE') {
            when { expression { BRANCH_NAME ==~ /(master|feature-.+|PR-.+)/ } }
            steps  {
                println "Deploy to PRE"
            }
        }
        stage('Deploy TEST') {
            when {
                allOf {
                    branch 'master'
                    expression {
                        params.deploymentTarget == 'TEST' ||
                                params.deploymentTarget == 'PP' ||
                                params.deploymentTarget == 'PROD'
                    }
                }
            }
            steps  {
                println "Deploy to TEST"
            }
        }
        stage('Deploy branch to PP') {
            when { expression { BRANCH_NAME ==~ /master/ } }
            steps {
                input "Promote to PP?"
                params.deploymentTarget = 'PP'
            }
        }
        stage('Deploy PP') {
            when {
                allOf {
                    branch 'master'
                    expression {
                        params.deploymentTarget == 'PP' ||
                                params.deploymentTarget == 'PROD'
                    }
                }
            }
            steps  {
                println "Deploy to PP"
            }
        }
        stage('Deploy branch to PROD') {
            when { expression { BRANCH_NAME ==~ /master/ } }
            steps {
                input "Promote to PROD?"
                params.deploymentTarget = 'PROD'
            }
        }
        stage('Deploy PROD') {
            when {
                allOf {
                    branch 'master'
                    expression {
                        params.deploymentTarget == 'PROD'
                    }
                }
            }
            steps  {
                println "Deploy to PROD"
            }
        }
    }
}
