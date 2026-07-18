pipeline {

    script {
        def testFunction() {
            def name = "Sivakumar"
            def age = 38
            def male = true
            def height = 5.8

            println name.class
            println age.class
            println male.class
            println height.class
        }
    }

    stages {
        stage 'Test' {
            steps {
                script {
                    testFunction()
                }
            }
        }
    }

}