@Library('jhenkins-shared-libs') _

pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                shell 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            steps {
                shell 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                shell 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    script {
                        def dest = 'dist/add2vals'
                        if ( ! isUnix()) {
                            dest += '.exe'
                        }
                        archiveArtifacts dest
                    }
                }
            }
        }
    }
}
