properties([
    pipelineTriggers([[$class: 'GitHubPushTrigger']])
])


pipeline {
    agent any
    environment {
        BRANCH = "${env.BRANCH_NAME}"
    }
    stages {
        stage('Create Virtual Environment') {
            steps {                
                    sh '''
                    echo $BRANCH
                    '''
                
            }
        }
        
        stage('Install Dependencies') {
            steps {
                withPythonEnv('python3'){
                    sh '''
                    pip install -r requirements.txt
                    '''
                }
            }
        }
        
        stage('Run Flask App') {
            steps {
                withPythonEnv('python3'){
                    // Run app.py in background on the virtual environment
                    sh 'nohup python app.py > app.log 2>&1 &'
                    // Optionally, wait for a few seconds to ensure the app has started
                    sleep 10
                }
            }
        }
        
    }
}
