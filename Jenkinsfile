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
        stage('Checkout') {
            steps {
                // Checkout your project's source code from your version control system (e.g., Git)
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                // Build your project with Maven
                bat 'mvn clean install' // Modify this command as needed
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image using the Dockerfile
                script {
                    def customImageTag = "my-app:${env.BUILD_NUMBER}" // Customize the image tag

                    // Build the Docker image with the Dockerfile in the project directory
                    docker.build(customImageTag, '.')

                    // Optionally, you can push the image to a Docker registry
                    // docker.withRegistry('https://registry.example.com', 'registryCredentials') {
                    //     docker.push(customImageTag)
                    // }
                }
            }
        }
    }

    post {
        success {
            // If everything is successful, you can perform additional actions here
            echo 'Build and Docker image creation successful!'
        }

        failure {
            // Handle failures or cleanup if needed
            echo 'Build or Docker image creation failed.'
        }
    }
}
