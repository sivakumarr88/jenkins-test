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

                        def testSafe = test ? test : "DEFAULT_VALUE"
                        echo "${testSafe}"

                        // Elvis - default values
                        def env_name = params.DEPLOY_ENV ?: "dev"
                        def branch = params.BRANCH_NAME ?: "main"
                        def retires = params.RETRIES ?: 3

                        // Safe navigation operator
                        def msg = env.GIT_COMMIT?.trim()

                        echo "env_name: ${env_name}, branch: ${branch}, retires: ${retires}, msg: ${msg}"

                        def mixedList = ["one", 2, true, 10.5]

                        echo "${mixedList[0]}" // prints one
                        for (item in mixedList) {
                            echo "${item}"
                        }

                        echo mixedList.join(" | ")

                        def mixedMap = [name: "Sivakumar", age: 38, male: true, height: 5.8]
                        echo "${mixedMap['name']}" // prints Sivakumar
                        for (entry in mixedMap) {
                            echo "${entry.key} : ${entry.value}"
                        }

                        def score = 75
                        if (score >= 90) {
                            echo "Grade: A"
                        } else if(score >=80) {
                            echo "Grade: B"
                        } else if(score >=70) {
                            echo "Grade: C"
                        } else {
                            echo "Grade: D"
                        }

                    }

                    testFunction()
                }
            }
        }
    }
}