//SCRIPTED




//DECLARATIVE

pipeline {
       agent any
      environment{
      registry = "kissbence599/maven-test"
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
                            script {
                               dockerImage = docker.build("kissbence599/maven-test:${env.BUILD_TAG}")
                                }
                            }
                        }
                        stage ('Run docker image') {
                            steps {
                            sh "docker run -d -p 8000:8000 kissbence599/maven-test:${env.BUILD_TAG}"
                            }
                        }
      }
      post {
            always {
                echo 'Im awsome. I run akéladnédasvlways'
            }
            success {
                echo 'I run when you are hehe'
            }
            failure {
                 echo 'I run when you faiil'
            }
      }

}
