pipeline {
    environment {
        registry = "khedijasidhom/devops"
        registryCredential = 'dockerhub_id'
        dockerImage = 'devops'
    }
    agent any
    stages {
        stage('Git') {
            steps {
                echo "Getting Project from Git";
                sh "rm -rf Exam"
                sh "git clone https://github.com/sidhomk/Devops.git"
                  }
            }
            
        stage('MVN Clean'){  
            steps {
                sh "mvn clean"
                  }
        }        
        
        stage('MVN Install'){
            steps {
                sh "mvn install"
                  }
        }       
        
        stage('MVN Test'){
            steps {
                sh "mvn test"
                  }
        }          
        
        stage('MVN Package'){
            steps {
                sh "mvn package"
                  }
        }          
        
        stage('MVN SONARQUBE'){
            steps{
                sh 'mvn sonar:sonar \
  -Dsonar.projectKey=devops \
  -Dsonar.host.url=http://192.168.1.196:9000 \
  -Dsonar.login=34d3e793fe1f7dee300da33aa08ef8858e10d6e2'
                 }
        }    
        stage('MVN NEXUS'){
            steps {
                sh 'mvn deploy -Dmaven.test.skip=true'
                  }
        }
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
   }
}
