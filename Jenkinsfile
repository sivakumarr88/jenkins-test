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

                        def testNull = null
                        echo "${testNull}" // prints null
                        echo testNull ? "testNull is not null" : "testNull is null" // prints testNull is null
                    }

                    testFunction()
                }
            }
        }
    }
}