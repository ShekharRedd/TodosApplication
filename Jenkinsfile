pipeline{
    agent any 
    environment{
        image2="todo-app"
        tag2="latest"
    }
    stages{
        stage("hello")
        {
            steps
            {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ShekharRedd/TodosApplication']])
            }
        }
        stage("Build the images "){
            when{
                expression{
                    
                }
            }
            steps{
                script{
                echo "========executing A========"
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                sh "cd /var/jenkins_home/workspace/build-jenkins-pipeline/FifthReact/ &&  docker build -t ${image2}:${tag2} ."
                sh 'echo $USER'
                sh "echo $PASS | docker login -u $USER --password-stdin"
                sh "docker tag ${image2}:${tag2} $USER/${image2}:${tag2}"
                sh "docker push $USER/${image2}:${tag2}"
                }                
            }
                }
            }
        }
        
    }

 

