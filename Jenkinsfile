// ============================================================
// BUGGY JENKINSFILE - Find & Fix All Issues!
// ============================================================

def printBanner(stageName) {
    echo '=============================='        // 🐛 BUG #1
    echo 'STAGE : ${stageName}'                 // 🐛 BUG #2
    echo '=============================='
}

def getDeployTag(branchName, buildNum) {
    if (branchName == 'main') {                 // 🐛 BUG #3
        return "release-${buildNum}"
    } else {
        return "snapshot-${buildNum}"
    }
}

pipeline {

    agent any

    environment {
        APP_NAME   = "my-app"
        BUILD_ENV  = "dev"
        MAX_RETRY  = 3
    }

    parameters {
        booleanParam(name: 'SKIP_TESTS',   defaultValue: false)
        booleanParam(name: 'SKIP_DEPLOY',  defaultValue: false)
        choice(name: 'DEPLOY_ENV', choices: ['dev', 'staging', 'prod'])
    }

    stages {

        // ─────────────────────────────────────────────
        // STAGE 1 - Variable Basics
        // ─────────────────────────────────────────────
        stage('Variable Basics') {
            steps {
                script {
                    def appVersion = "1.0.0"        // 🐛 BUG #4 - moved into script block
                    echo "Version: ${appVersion}"
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 2 - String & Variable Reference
        // ─────────────────────────────────────────────
        stage('String and Variable Reference') {
            steps {
                script {
                    def buildLabel  = 'Build-${env.BUILD_NUMBER}'   // 🐛 BUG #5
                    def deployMsg   = "Deploying ${APP_NAME}"       // 🐛 BUG #6
                    def skipTests   = params.SKIP_TESTS

                    echo "Label   : ${buildLabel}"
                    echo "Message : ${deployMsg}"
                    echo "Skip    : ${skipTests}"
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 3 - Shell Variable Reference
        // ─────────────────────────────────────────────
        stage('Shell Variable Reference') {
            steps {
                script {
                    def groovyVar = "I-am-groovy"

                    // passing groovy var into shell
                    sh 'echo Groovy Var is: ${groovyVar}'           // 🐛 BUG #7

                    // pure shell variable
                    sh """
                        SHELL_VAR="I-am-shell"
                        echo "Shell Var is: \$SHELL_VAR"             // 🐛 BUG #8
                    """

                    // capture shell output back to groovy
                    def gitCommit = sh("git rev-parse --short HEAD") // 🐛 BUG #9
                    echo "Git Commit: ${gitCommit}"
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 4 - Null Safety & Elvis
        // ─────────────────────────────────────────────
        stage('Null Safety') {
            steps {
                script {
                    def gitTag = env.GIT_TAG                        // might be null

                    def tagMessage = gitTag?.toUpperCase()           // 🐛 BUG #10

                    def releaseType = gitTag ? "release" : "snapshot"

                    echo "Tag     : ${gitTag}"
                    echo "Message : ${tagMessage}"
                    echo "Type    : ${releaseType}"
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 5 - Collections
        // ─────────────────────────────────────────────
        stage('Collections') {
            steps {
                script {
                    def services = ["auth-service", "user-service", "order-service"]
                    def envConfig = [
                        dev     : [replicas: 1, memory: "512Mi"],
                        staging : [replicas: 2, memory: "1Gi"],
                        prod    : [replicas: 3, memory: "2Gi"]
                    ]

                    // access last service
                    echo "Last Service : ${services[3]}"            // 🐛 BUG #11

                    // access nested map
                    def selectedConfig = envConfig[params.DEPLOY_ENV]
                    echo "Replicas : ${selectedConfig.replicas}"
                    echo "Memory   : ${selectedConfig.memory}"

                    // filter services
                    def shortNames = services.findAll { it.length() < 10 }  // 🐛 BUG #12 (logical)
                    echo "Short named services: ${shortNames}"
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 6 - Loops
        // ─────────────────────────────────────────────
        stage('Loops') {
            steps {
                script {
                    def services  = ["auth-service", "user-service", "order-service"]
                    def envConfig = [dev: "dev-server", staging: "stg-server"]

                    // loop over list
                    for (service in services) {
                        sh 'echo Building ${service}'               // 🐛 BUG #13
                    }

                    // loop over map
                    envConfig.each { envName, server ->
                        echo "Env: ${envName} → Server: ${server}"
                    }
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 7 - Conditionals
        // ─────────────────────────────────────────────
        stage('Conditionals') {
            steps {
                script {
                    def branch = env.BRANCH_NAME

                    if (branch == "main") {                          // 🐛 BUG #14
                        echo "Main branch - full build"
                        sh "echo running full build"
                    } else {
                        echo "Other branch - quick build"
                        sh "echo running quick build"
                    }

                    def retries = env.MAX_RETRY
                    if (retries > 2) {                              // 🐛 BUG #15
                        echo "Retries enabled: ${retries}"
                    }
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 8 - Functions
        // ─────────────────────────────────────────────
        stage('Functions') {
            steps {
                script {
                    printBanner("Functions Stage")                  // refers to BUG #1 #2

                    def tag = getDeployTag(env.BRANCH_NAME, env.BUILD_NUMBER)
                    echo "Tag: ${tag}"                              // refers to BUG #3
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 9 - Exception Handling
        // ─────────────────────────────────────────────
        stage('Exception Handling') {
            steps {
                script {
                    catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                        echo "Trying risky operation..."
                        sh 'cat non-existent-file.txt'
                    }
                    echo "Completed risky operation (errors handled)"
                }
            }
        }

        // ─────────────────────────────────────────────
        // STAGE 10 - Deploy (when condition)
        // ─────────────────────────────────────────────
        stage('Deploy') {
            when {
                expression { params.SKIP_DEPLOY == false }          // 🐛 BUG #17
            }
            steps {
                script {
                    def tag = getDeployTag(env.BRANCH_NAME, env.BUILD_NUMBER)
                    echo "Deploying ${env.APP_NAME} with tag ${tag}"
                    sh "echo deploying to ${env.DEPLOY_ENV}"       // 🐛 BUG #18
                }
            }
        }

    }

    post {
        always {
            echo "Build #${BUILD_NUMBER} finished : ${currentBuild.currentResult}"  // 🐛 BUG #19
        }
        success {
            script {
                def services = ["auth-service", "user-service", "order-service"]
                def deployed = ""
                services.each { svc ->
                    deployed = deployed + svc + ", "
                }
                echo "Deployed: ${deployed}"                        // 🐛 BUG #20 (logical)
            }
        }
    }
}