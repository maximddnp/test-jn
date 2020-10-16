pipeline {

    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '15'))
    }

    parameters {
        choice(name: 'deploymentTarget', choices: [ 'PRE', 'TEST', 'PP', 'PROD' ], description: 'Env for deploy?')
    }


    stages {
        stage('Print') {
            steps {
                script {
                    sh 'printenv'
                }
            }
        }

        stage('Deploy PRE') {
            when { expression { GIT_BRANCH ==~ /main/ } }
            steps  {
                println "Deploy to PRE"
            }
        }
        stage('Deploy branch to TEST?') {
            when {
                allOf {
                    branch 'main'
                    expression {
                        params.deploymentTarget != 'TEST' ||
                                params.deploymentTarget != 'PP' ||
                                params.deploymentTarget != 'PROD'
                    }
                }
            }
            steps {
                script {
                    input "Promote to TEST?"
                    deploymentTarget = 'TEST'
                }
            }
        }
        stage('Deploy TEST') {
            when {
                allOf {
                    branch 'main'
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
            when {
                allOf {
                    branch 'main'
                    expression {
                        params.deploymentTarget != 'PP' ||
                                params.deploymentTarget != 'PROD'
                    }
                }
            }
            steps {
                script {
                    input "Promote to PP?"
                    deploymentTarget = 'PP'
                }
            }
        }
        stage('Deploy PP') {
            when {
                allOf {
                    branch 'main'
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
            when {
                allOf {
                    branch 'main'
                    expression {
                        params.deploymentTarget != 'PROD'
                    }
                }
            }
            steps {
                script {
                    input "Promote to PROD?"
                    deploymentTarget = 'PROD'
                }
            }
        }
        stage('Deploy PROD') {
            when {
                allOf {
                    branch 'main'
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
