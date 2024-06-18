properties([
    pipelineTriggers([[$class: 'GitHubPushTrigger']])
])


pipeline {
    agent any
    
    stages {
        stage('Create Virtual Environment') {
            steps {
                script {
                    // Create a virtual environment
                    sh 'python3 -m venv venv'
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    // Activate the virtual environment and install dependencies
                    sh 'source venv/bin/activate && pip install -r requirements.txt'
                }
            }
        }
        
        stage('Run Flask App') {
            steps {
                script {
                    // Run app.py in background on the virtual environment
                    sh 'source venv/bin/activate && nohup python app.py > app.log 2>&1 &'
                    // Optionally, wait for a few seconds to ensure the app has started
                    sleep 10
                }
            }
        }
        
        stage('Clean Workspace') {
            steps {
                script {
                    // Clean up workspace
                    deleteDir()
                }
            }
        }
    }
    
    post {
        always {
            // Clean up virtual environment after the pipeline finishes
            script {
                sh 'deactivate || :'
                deleteDir()
            }
        }
    }
}