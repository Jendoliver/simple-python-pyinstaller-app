pipeline {
    agent any
    stages {
        def shell = isUnix() ? { sh } : { bat }
        stage('Build') {
            steps {
                script {
                    shell 'python -m py_compile sources/add2vals.py sources/calc.py'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    shell 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
                }
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }
}
