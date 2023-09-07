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
    stages {

        stage('SCM Preparation') {
            steps {
                echo "BranchName: ${Main}"
                echo "Code Update Started"
                git branch: "${Main}", url: 'https://github.com/VenkataManeesh/DevOps_Project.git'
                echo "Code Update End"
            }
        }
        
        stage('Clean') {
            steps {
                echo "Clean Started"
                bat(/mvn -file spring-boot-jwt\pom.xml clean/)
                echo "Clean End"
            }
        }

        stage('Compile') {
            steps {
                echo "Code Compilation Started"
                bat(/mvn -file spring-boot-jwt\pom.xml compile/)
                echo "Code Compilation End"
            }
        }
        

        
        stage('Build Image') {
            steps {
                echo "Build Image Started"
                bat(/mvn -file spring-boot-jwt\pom.xml package -Dmaven.test.skip=true/)
                bat(/docker build --build-arg VER=${jarVersion} -f spring-boot-jwt\dockerfile -t spring\/spring-boot-jwt:${tagName} ./)
                echo "Build Image Started"
            }
        }
    }
