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

        // stage("checkout feature branch"){


        //     steps{
        //         // echo "executing unit and integration test"
        //         dir('/var/jenkins_home/workspace/build-jenkins-pipeline/'){
        //             echo "hello world"
        //             sh "git checkout origin feature"
        //             // sh "npm install"
        //         // def testExitCode = sh(script: 'npm test', returnStatus: true)
        //         // if (testExitCode == 0) {
        //         //     echo "Tests passed. Proceeding to the next stage."
        //         //             // Add further steps for deployment, etc.
        //         //     } else {
        //         //         error "Tests failed. Aborting pipeline."
        //         //         }
        //         // }
        //     }
        // }

        stage("Checkout and Merge Feature Branch") {
            steps {
                script {
                    dir('/var/jenkins_home/workspace/build-jenkins-pipeline') {
                        sh "git checkout main"
                        sh "git pull origin main"

                        // Attempt to merge the feature branch
                        def mergeExitCode = sh(script: 'git merge feature', returnStatus: true)
                        
                        if (mergeExitCode == 0) {
                            echo "Merge successful. Committing changes and pushing to main."
                            sh "git add ."
                            sh 'git commit -m "merging from feature branch"'
                            sh "git push origin main"
                        } else {
                            error "Merge failed. Please resolve conflicts manually and try again."
                            // Add any additional actions or notifications here
                        }
                    }
                }
            }
        }
        stage("Building docker image"){
            steps{
                script{
                dir('/var/jenkins_home/workspace/build-jenkins-pipeline/'){
                echo "========executing A========"
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                sh "git checkout main"
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
        
    }


 

