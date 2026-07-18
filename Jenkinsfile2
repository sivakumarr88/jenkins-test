pipeline {
    agent any
    environment {
        APP_NAME    = "my-app"
        BUILD_ENV   = "dev"
    }

    parameters {
        string(name: 'GREETING', defaultValue: 'Hello, World!', description: 'Greeting message')
    }

    stages {
        stage('Print Greeting') {
            steps {
                script {
                    // Accessing the parameter
                    def greetingMessage = params.GREETING
                    echo "Greeting: ${greetingMessage}"
                    echo "Deploying ${env.App_NAME} in ${env.BUILD_ENV} environment"

                    sh """
                        echo "Greeting from shell: ${greetingMessage}"
                        echo "App Name from shell: ${env.APP_NAME}"
                        echo "Build Environment from shell: ${env.BUILD_ENV}"

                        SHELL_VAR="I am a shell variable"

                        echo "Shell Variable: \${SHELL_VAR}"
                    """
                }

                script {
                    def groovyVar = "I am a Groovy variable"
                    echo "Groovy Variable: ${groovyVar}"
                }
            }
        }
    }
}