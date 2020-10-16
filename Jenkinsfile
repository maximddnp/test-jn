def envDeploy
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
                script {
                    envDeploy = params.deploymentTarget
                    println "Deploy to PRE"
                }
            }
        }
        stage('Deploy branch to TEST?') {
            when {
                allOf {
                    branch 'main'
                    expression {
                        envDeploy != 'TEST'
                    }
                    expression {
                        envDeploy != 'PP'
                    }
                    expression {
                        envDeploy != 'PROD'
                    }
                }
            }
            steps {
                script {
                    input "Promote to TEST?"
                    envDeploy = 'TEST'
                    println "$envDeploy"
                }
            }
        }
        stage('Deploy TEST') {
            when {
                allOf {
                    branch 'main'
                    expression {
                        envDeploy == 'TEST' ||
                                envDeploy == 'PP' ||
                                envDeploy == 'PROD'
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
                        envDeploy != 'PP'
                    }
                    expression {
                        envDeploy != 'PROD'
                    }
                }
            }
            steps {
                script {
                    input "Promote to PP?"
                    envDeploy = 'PP'
                }
            }
        }
        stage('Deploy PP') {
            when {
                allOf {
                    branch 'main'
                    expression {
                        envDeploy == 'PP' ||
                                envDeploy == 'PROD'
                    }
                }
            }
            steps  {
                println "Deploy to PP"
            }
        }
        stage('Deploy branch to PROD?') {
            when {
                allOf {
                    branch 'main'
                    expression {
                        envDeploy != 'PROD'
                    }
                }
            }
            steps {
                script {
                    input "Promote to PROD?"
                    envDeploy = 'PROD'
                }
            }
        }
        stage('Deploy PROD') {
            when {
                allOf {
                    branch 'main'
                    expression {
                        envDeploy == 'PROD'
                    }
                }
            }
            steps  {
                println "Deploy to PROD"
            }
        }
    }
}
