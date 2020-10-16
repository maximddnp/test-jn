pipeline {

    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '15'))
    }

    parameters {
        choice(name: 'deploymentTarget', choices: [ 'PROD', 'PRE', 'TEST', 'PP' ], description: 'Env for deploy?')
    }


    stages {

        stage('Deploy PRE') {
            when { expression { GIT_BRANCH ==~ /(master|feature-.+|PR-.+)/ } }
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
        stage('Deploy branch to PP?') {
            when { expression { GIT_BRANCH ==~ /master/ } }
            steps {
                script {
                    input "Promote to PP?"
                }
            }
        }
        stage('Deploy PP') {
            when {
                allOf {
                    GIT_BRANCH 'master'
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
                script {
                    input "Promote to PROD?"
                }
            }
        }
        stage('Deploy PROD') {
            when {
                allOf {
                    GIT_BRANCH 'master'
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
