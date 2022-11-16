pipeline{
    agent any
    tools {
        maven 'M2_HOME'
    }


    stages {


        /*stage('preparation : start sonar, nexus') {
            steps{
                	sh "docker start d94"
                	sh "docker start 0f3"
            }
        }*/


        stage('Getting project from Git') {
            steps{
      			checkout([$class: 'GitSCM', branches: [[name: '*/Sinda']],
			extensions: [],
			userRemoteConfigs: [[url: 'https://github.com/DevopsSIM3-2022/Spring.git']]])
            }
        }



        stage('Cleaning the project') {
            steps{
                	sh "mvn -B -DskipTests clean"
                    sh "mvn install "
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

             		sh " mvn sonar:sonar -Dsonar.projectKey=backspring -Dsonar.host.url=http://localhost:9000 -Dsonar.login=09663dd7741f2053e9ce7522847373131857454d"

            }
        }


        stage('Publish to Nexus') { 
            steps {


         sh 'mvn clean package deploy:deploy-file -DgroupId=tn.esprit.rh -DartifactId=achat -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://localhost:8081/repository/maven-releases/ -Dfile=target/achat-1.0.jar'


            }
        }

stage('Build Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t 07495014/spring-app:latest .'
                          }
                      }
                  }

                  stage('login dockerhub') {
                                        steps {
                                      sh 'docker login -u 07495014 -p dckr_pat_IGzrU64ntM8f_dw9HPZ9Wh0OcMk'
                            
		  }
	    
	                      stage('Push Docker Image') {
                                        steps {
                                   sh 'docker push 07495014/spring-app:latest'
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

	    
        post {
        always {
            cleanWs()
        }
    }

    
	
}