@Library("Shared") _

pipeline {
    agent {
        label "agent"
    }
    stages{
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Code"){
            steps{
                 script{
                    code_checkout("https://github.com/Hardy121/node-sample-project.git", "main")
                }
            }
        }
        stage("Build"){
            steps{
                script{
                    echo "This is building the code"
                    docker_build("sample-node-project","latest", "hardik002")
                }
            }   
        }
        stage("Push code on dockerhub"){
            steps{
                echo "This is pushing the code"
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerHubCred",
                        passwordVariable: "dockerHubPass",
                        usernameVariable: "dockerHubUser"
                    )    
                ]){
                sh '''  
                    echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                    docker image tag sample-node-project:latest "$dockerHubUser/sample-node-project:latest"
                    docker push "$dockerHubUser/sample-node-project:latest"
                ''' 
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "This is docker compose up"
                sh 'docker compose up -d'
            }
        }
    }
}
