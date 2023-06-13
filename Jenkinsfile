pipeline {
      agent any
      environment {
            MY_CREDENTIALS = credentials('f995695c-89c3-4bdb-88ae-85bb71c0cc71') // Retrieve Jenkins credentials
      }
      stages {
                  stage('testing') {
                        steps {
                        echo 'Hi, this is testing stage for credentials'

                        script {
                              sh "echo 'My credentials: ${env.MY_CREDENTIALS}'"
                        }
                        }
                  }
            stage('One') {
                  steps {
                        echo 'Hi, this is Haseeb experimenting with Jenkins'
                  }
            }
            stage('Two') {
                  steps {
                        input('Do you want to proceed?')
                  }
            }
            stage('Three') {
                  when {
                        not {
                              branch 'master'
                        }
                  }
                  steps {
                        echo 'Hello'
                  }
            }
            stage('Four') {
                  parallel {
                        stage('Unit Test') {
                              steps {
                                    echo 'Running the unit test...'
                              }
                        }
                        stage('Integration test') {
                              steps {
                                    echo 'Running the integration test...'
                              }
                        }
                  }
            }
      }
}
