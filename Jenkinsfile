/*
Ian Macdonald
2025/04/14
Jenkinsfile for 4850 final exam 
source code is sample code 3

*/

pipeline {
    agent { label 'java_agent' }

    parameters {
        booleanParam(defaultValue: false, description: 'Whether to Run the Code', name: 'RUN')
        string(defaultValue: "Development", description: 'Type of build', name: 'BUILD_TYPE')
    }

    stages {
        stage('Setup') {
            steps {
                echo "Build number is ${env.BUILD_ID} with workspace: ${env.WORKSPACE}"
                echo "Ian Macdonald A01348901 Group 42"
            }
        }
        stage('Build') {
            steps {
                echo "Starting Java Build..." 
                sh 'mvn -B -DskipTests clean install'
                echo "Java Build Complete."
            }
        }
        stage('Code Quantity') {
            
            steps {
                /*
                Stage called Code Quantity that counts and prints to the console the number of 
                lines in the App.java file. It must also use a Groovy for-loop to display the name of 
                each file in the repo (including sub-folders) to the console.

                For the Code Quantity Stage, you can use the "wc -l" command to get the number of 
                lines in a file. You may want to Google this command or use the Linux man pages.
                */

                sh "wc -l ./src/main/java/com/mycompany/app/App.java"

                script{
                    def files = findFiles()
                    for (file in files) {
                        echo "${file} is a file in the repo"
                    }
                }
            }

        }
        stage('Test') {
            /*

            publishes the junit test results 
            (so the graph of unit test results shows up in the Jenkins job) only if the 
            BUILD_TYPE is equal to "Development".

            */
            
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    script {
                        if ( { params.BUILD_TYPE } == "Development"){
                            junit 'target/surefire-reports/*.xml'
                        }
                    }
                }
            }
        }

        stage('Run') {
            when {
                expression { params.RUN }
            }
            steps {
                sh 'deliver.sh'
            }
        }

        stage('Build Results') {
            steps {
                echo "Build ${params.BUILD_TYPE} completed successfully"
                echo "I have now completed ACIT 4850!"
            }
        }
    }
}

