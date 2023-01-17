pipeline {
    agent { label 'test' }
    environment {
          max = 10
          rand = "${Math.abs(new Random().nextInt(max+1))}"
    }
    stages {
        stage('RunTest?') {
            steps {
                script {
                    echo "${rand}"
                    if ("${rand}/2" == "0") {
                        echo 'Success'
                    } else {
                        echo 'Test failed'
                    }
                }
            }
        }
    }
}
