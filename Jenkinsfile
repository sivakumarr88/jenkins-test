pipeline {

    agent any

    // -------------------------------------------------------
    // Jenkins Environment Variables
    // Accessible everywhere using env.VAR_NAME
    // -------------------------------------------------------
    environment {
        APP_NAME    = "my-app"
        BUILD_ENV   = "dev"
    }

    parameters {
        booleanParam(name: 'SKIP_DEPLOY', defaultValue: false, description: 'Skip deployment?')
    }

    // =======================================================
    stages {

        // ---------------------------------------------------
        // CONCEPT 1 & 2: Groovy Variable + Capture shell output → Groovy
        // ---------------------------------------------------
        stage('Setup') {
            steps {
                script {

                    // Groovy variable
                    def greeting = "Hello from Jenkins!"
                    echo "${greeting}"                           // ✅ double quotes = interpolation
                    echo '${greeting}'                          // ❌ single quotes = printed as-is (literal)

                    // Capture shell output → into Groovy variable
                    def gitCommit = sh(
                        script: "git rev-parse --short HEAD",
                        returnStdout: true
                    ).trim()

                    echo "Git Commit is: ${gitCommit}"          // Groovy var in echo

                    // Promote Groovy var → env var so other stages can access it
                    env.GIT_COMMIT_SHORT = gitCommit

                }
            }
        }

        // ---------------------------------------------------
        // CONCEPT 2: Referencing Variables in shell (sh)
        // ---------------------------------------------------
        stage('Variable Reference in Shell') {
            steps {
                script {

                    def groovyVar = "I am Groovy"

                    // Groovy var → inside shell using double quotes
                    // Groovy resolves ${groovyVar} BEFORE shell runs
                    sh "echo Groovy says: ${groovyVar}"

                    // Jenkins env var → inside shell
                    sh "echo App is: ${env.APP_NAME}"

                    // Pure shell - single quotes, Groovy does NOT touch it
                    // Shell handles $ on its own
                    sh '''
                        SHELL_VAR="I am Shell"
                        echo "Shell says: $SHELL_VAR"
                    '''

                    // Mixed: Groovy var + Shell var in same block
                    // Use double quotes, escape shell $ with \$
                    sh """
                        SHELL_VAR="I am Shell"
                        echo "Groovy: ${groovyVar}"
                        echo "Shell : \$SHELL_VAR"
                        echo "Env   : ${env.APP_NAME}"
                    """

                }
            }
        }

        // ---------------------------------------------------
        // CONCEPT 3: script{} block - Groovy + Shell together
        // ---------------------------------------------------
        stage('Script Block Usage') {
            steps {

                // Jenkins DSL step - no script{} needed
                echo "This is a plain Jenkins DSL step"
                sh "echo This is a plain shell step"

                // script{} block → to write Groovy logic
                script {

                    // Groovy code here
                    def fileName = "build.log"

                    // Shell inside Groovy inside script{}
                    sh "touch ${fileName}"
                    sh "echo 'build started' > ${fileName}"

                    // Capture shell output back to Groovy
                    def fileContent = sh(
                        script: "cat ${fileName}",
                        returnStdout: true
                    ).trim()

                    echo "File contains: ${fileContent}"        // Back in Groovy world

                }
            }
        }

        // ---------------------------------------------------
        // CONCEPT 4a: if/else - Groovy (pipeline flow decisions)
        // ---------------------------------------------------
        stage('Groovy if-else') {
            steps {
                script {

                    def branch = env.BRANCH_NAME ?: "main"     // ?: is Groovy elvis operator (default if null)

                    // Groovy if/else → decides WHAT pipeline should do
                    if (branch == "main") {
                        echo "Main branch → will run full build"
                        sh "echo Running full build"
                    } else if (branch == "develop") {
                        echo "Develop branch → will run dev build"
                        sh "echo Running dev build"
                    } else {
                        echo "Feature branch → run quick check only"
                        sh "echo Running quick check"
                    }

                }
            }
        }

        // ---------------------------------------------------
        // CONCEPT 4b: if/else - Shell (OS / file level decisions)
        // ---------------------------------------------------
        stage('Shell if-else') {
            steps {

                // Shell if/else → decides OS level actions
                sh '''
                    if [ -f "pom.xml" ]; then
                        echo "Maven project detected"
                    elif [ -f "build.gradle" ]; then
                        echo "Gradle project detected"
                    else
                        echo "Unknown project type"
                    fi
                '''

            }
        }

        // ---------------------------------------------------
        // CONCEPT 4c: Loops - Groovy for loop
        // ---------------------------------------------------
        stage('Groovy Loop') {
            steps {
                script {

                    // Groovy List
                    def services = ["auth-service", "user-service", "order-service"]

                    // Groovy for loop → run shell command for each item
                    for (service in services) {
                        echo "Processing: ${service}"
                        sh "echo Building ${service}"           // Groovy var passed into shell
                    }

                    // Groovy Map
                    def envConfig = [dev: "dev-server", staging: "staging-server"]

                    // Groovy .each{} loop on map
                    envConfig.each { envName, server ->
                        echo "Env: ${envName} → Server: ${server}"
                    }

                }
            }
        }

        // ---------------------------------------------------
        // CONCEPT 4d: Loops - Shell for loop
        // ---------------------------------------------------
        stage('Shell Loop') {
            steps {

                // Shell for loop → purely OS level iteration
                sh '''
                    for FILE in *.xml; do
                        echo "Found file: $FILE"
                    done
                '''

            }
        }

        // ---------------------------------------------------
        // CONCEPT: when{} - Groovy expression to skip a stage
        // ---------------------------------------------------
        stage('Deploy') {
            when {
                expression { return !params.SKIP_DEPLOY }
            }
            steps {
                echo "Deploying ${env.APP_NAME} to ${env.BUILD_ENV}"
                sh "echo Deployment done!"
            }
        }

    }
    // =======================================================

    // -------------------------------------------------------
    // POST - Groovy if/else based on build result
    // -------------------------------------------------------
    post {
        success { echo "✅ Pipeline PASSED!" }
        failure { echo "❌ Pipeline FAILED!" }
        always  { echo "Build #${env.BUILD_NUMBER} finished with: ${currentBuild.currentResult}" }
    }

}