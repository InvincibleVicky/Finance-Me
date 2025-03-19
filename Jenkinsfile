pipeline {
    agent any
    
    stages {
        stage("Git Checkout") {
            steps {
                echo "Cloning Git Repo"
                git url: "https://github.com/InvincibleVicky/Finance-Me/"
            }
        }

        stage("Compile the Code") {
            steps {
                echo "Starting Compilation"
                sh "mvn compile"
            }
        }
        
        stage("Test the Code") {
            steps {
                echo "Starting Testing"
                sh "mvn test"
            }
        }

        stage("QA of the Code") {
            steps {
                echo "Starting Compilation"
                sh "mvn checkstyle:checkstyle"
            }
        }

        stage("Create a Package") {
            steps {
                echo "Packaging Application"
                sh "mvn package"
            }
        }

        stage('Create a Docker image from the Package Insure-Me.jar file') {
            steps {
                sh 'docker build -t vigneshwar1908/finance-me:v1 .'
            }
        }
        
        stage('Login to Dockerhub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerlogin', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
                sh 'docker login -u ${dockeruser} -p ${dockerpass}'                                                       }
            }
        }
        
        stage('Push the Docker image') {
            steps {
                sh 'docker push vigneshwar1908/finance-me:v1'
            }
        }

        stage('Ansbile config and Deployment') {
            steps {
                ansiblePlaybook credentialsId: 'ansible-ssh',
                    disableHostKeyChecking: true,
                    installation: 'ansible',
                    inventory: '/etc/ansible/hosts', 
                    playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
}
