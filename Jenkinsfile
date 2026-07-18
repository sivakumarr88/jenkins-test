pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                script {
                    def testFunction = {
                        def name = "Sivakumar"
                        def age = 38
                        def male = true
                        def height = 5.8

                        println name.class
                        println age.class
                        println male.class
                        println height.class

                        echo 'Name is ${name} (printed with single quote)'
                        echo "Name is ${name} (printed with double quote)"
                        echo '''
                            Age is ${age} (printed with triple single quote)
                            Male is ${male} (printed with triple single quote)
                            Height is ${height} (printed with triple single quote)
                        '''
                        echo """
                            Age is ${age} (printed with triple single quote)
                            Male is ${male} (printed with triple single quote)
                            Height is ${height} (printed with triple single quote)
                        """

                        def test = null
                        echo "${test}" // prints null
                        echo test ? "test is not null" : "test is null" // prints test is null

                        def GROOVY_VAR = "GROOVY_VAR"
                        // Triple single quote → pure shell (no Groovy vars are expanded)
                        sh '''
                            SHELL_VAR="SHELL_VAR"
                            echo "GROOVY_VAR is ${GROOVY_VAR} (printed from shell script)"
                            echo "SHELL_VAR is ${SHELL_VAR} (printed from shell script)"
                        '''

                        // Triple double quote → shell + Groovy vars mixed, Groovy vars are expanded
                        sh """
                            SHELL_VAR1="SHELL_VAR1"
                            echo "GROOVY_VAR is ${GROOVY_VAR} (printed from shell script)"
                            echo "SHELL_VAR1 is \${SHELL_VAR1} (printed from shell script)"
                        """
                    }

                    testFunction()
                }
            }
        }
    }
}