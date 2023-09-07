// pipeline {
// 	agent none
//   stages {
//   	stage('Maven Install') {
// 		agent any
//       steps {
//       	sh 'mvn clean install'
//       }
//     }
//     stage('Docker Build') {
//     	agent any
//       steps {
//       	sh 'docker build -t spring-k8s-example:latest .'
//       }
//     }
//   }
// }

pipeline {
    agent any 
    environment {
        Main = 'Main' // Specify your desired branch name here with the correct case
        jarVersion = '1.0' // Specify your desired jar version here
        tagName = 'latest' // Specify your desired Docker image tag here
    }
    stages {

        stage('SCM Preparation') {
            steps {
                echo "BranchName: ${Main}"
                echo "Code Update Started"
                checkout([$class: 'GitSCM',
                          branches: [[name: "${Main}"]],
                          userRemoteConfigs: [[url: 'https://github.com/VenkataManeesh/DevOps_Project.git']]])
                echo "Code Update End"
            }
        }
        
        stage('Clean') {
            steps {
                echo "Clean Started"
                sh 'mvn clean -f spring-boot-jwt/pom.xml'
                echo "Clean End"
            }
        }

        stage('Compile') {
            steps {
                echo "Code Compilation Started"
                sh 'mvn compile -f spring-boot-jwt/pom.xml'
                echo "Code Compilation End"
            }
        }
        
        stage('Build Image') {
            steps {
                echo "Build Image Started"
                sh 'mvn package -f spring-boot-jwt/pom.xml -Dmaven.test.skip=true'
                sh "docker build --build-arg VER=${jarVersion} -f spring-boot-jwt/dockerfile -t spring/spring-boot-jwt:${tagName} ./"
                echo "Build Image Started"
            }
        }
    }
}
