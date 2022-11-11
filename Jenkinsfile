pipeline{
    agent any
    tools {
        maven 'M2_HOME'
    }


    stages {


 /*stage('preparation : start sonar, nexus') {
            steps{
                	sh "docker start 5f9"
                	sh "docker start 5ed"
            }
        }*/


        stage('Getting project from Git') {
            steps{
      			checkout([$class: 'GitSCM', branches: [[name: '*/Aziz']],
			extensions: [],
			userRemoteConfigs: [[url: 'https://github.com/DevopsSIM3-2022/Spring.git']]])
            }
        }



        stage('Cleaning the project') {
            steps{
                	sh "mvn -B -DskipTests clean  "
                    sh "mvn install  "
            }
        }



        stage('Artifact Construction') {
            steps{
                	sh "mvn -B -DskipTests package "
            }
        }



         stage('Unit Tests') {
            steps{
               		 sh "mvn test "
            }
        }


        stage('Code Quality Check via SonarQube') {
            steps{

                    sh " mvn sonar:sonar -Dsonar.projectKey=backspring -Dsonar.host.url=http://localhost:9000 -Dsonar.login=1a5328b57bc34694e8cf96784404bc0f1df0dae8"
             		//sh " mvn sonar:sonar -Dsonar.projectKey=backspring -Dsonar.host.url=http://localhost:9000 -Dsonar.login=d76baec792c8b69cb7aa6b22ee2f97efd83860b0"

            }
        }

        stage('Publish to Nexus') { 
            steps {


         sh 'mvn package deploy:deploy-file -DgroupId=tn.esprit.rh -DartifactId=achat -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://192.168.1.31:8081/repository/maven-releases/ -Dfile=target/achat-1.0.jar'


            }
        }

stage('Build Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t status404/spring-app:latest .'
                          }
                      }
                  }

                  stage('login dockerhub') {
                                        steps {
                                      sh 'docker login -u status404 -p dckr_pat_FuLeu62QKYn3b7vH1kumw_vwrTk'
                                            }
		  }
	    
	                      stage('Push Docker Image') {
                                        steps {
                                   sh 'docker push status404/spring-app:latest'
                                            }
		  }


		   stage('Run Spring && MySQL Containers') {
                                steps {
                                    script {
                                      sh 'docker-compose up -d'
                                    }
                                }
                            }
     
}

	/*    
        post {
        always {
            cleanWs()
        }
    }
*/
    
	
}