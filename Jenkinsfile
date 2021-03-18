//SCRIPTED




//DECLARATIVE

pipeline {
       agent any
      // agent { docker { image 'maven:latest' }  }
      // agent { docker { image 'node:13.8' }  }
      environment{
      registry = "csabaazari/currency-exchange-devops11"
      dockerHome = tool 'myDocker'
      mavenHome = tool 'myMaven'
      PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"

      }



        stages {
            stage('Checkout') {
                steps {
                       sh 'mvn --version'
                       sh 'docker --version'

                      echo "Build"
                      echo "PATH - $PATH"
                      echo "BUILD_NUMBER - $env.BUILD_NUMBER"
                      echo "BUILD_ID - $env.BUILD_ID"
                      echo "JOB_NAME - $env.JOB_NAME"
                      echo "BUILD_TAG - $env.BUILD_TAG"
                      echo "BUILD_URL - $env.BUILD_URL"
                }
            }

                        stage('Compile'){
                            steps {
                                sh "mvn clean compile"


                            }


                        }


                        stage('Test') {
                            steps {
                            	sh "mvn test"
                            }
                        }
                                    stage('Integration Test') {
                                        steps {
                                            sh "mvn failsafe:integration-test failsafe:verify"
                                        }
                                    }
                                    stage('Package') {
                                            steps {
                                              sh "mvn package -DskipTests"
                             }
                        }



                        stage ('Build docker image') {
                            steps {
                            //"docker build -t csabaazari/currency-exchange-devops:$env.BUILD_TAG"
                            script {
                               dockerImage = docker.build("csabaazari/currency-exchange-devops11:${env.BUILD_TAG}")

                             //  dockerImage = docker.run ("csabaazari/currency-exchange-devops11:${env.BUILD_TAG}")
                                }
                            }
                        }
                        stage ('Run docker image') {
                            steps {
                            sh "docker run -d -p 8000:8000 csabaazari/currency-exchange-devops11:${env.BUILD_TAG}"
                            }
                        }
      }
      post {
            always {
                echo 'Im awsome. I run always'
            }
            success {
                echo 'I run when you are succesful'
            }
            failure {
                 echo 'I run when you fail'
            }
      }

}
