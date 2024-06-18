properties([
    pipelineTriggers([[$class: 'GitHubPushTrigger']])
])


pipeline {
    agent any
    
    stages {
        stage('Create Virtual Environment') {
            steps {
                withPythonEnv('python3.9'){
                    sh '''
                    pip install --upgrade pip
                    '''
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                withPythonEnv('python3.9'){
                    sh '''
                    pip install -r requirements.txt
                    '''
                }
            }
        }
        
        stage('Run Flask App') {
            steps {
                withPythonEnv('python3.9'){
                    // Run app.py in background on the virtual environment
                    sh 'nohup python app.py > app.log 2>&1 &'
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