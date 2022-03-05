pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Build') {
            steps {
                bat(script: 'docker images -a')
                bat(script: """
                    cd azure-vote/
                    docker images -a
                    docker build -t jenkins-pipeline .
                    docker images -a
                    cd ..
                """)
            }

        stage('Start test app') {
            steps {
                bat(script: """
                    docker-compose up -d
                    ./scripts/test_container.ps1
                    """)
            }
            post {
                success {
                    echo "App started successfully :)"
                }
                failure {
                    echo "App failed to start :("
                }
            }
        stage ('Run Tests') {
            steps {
                bat(script: """
                    pytest ./tests/test_sample.py
                """)
            }
            }
        stage ('Stop test app') {
            steps {
                bat(script: """
                    docker-compose down
                """)
            }
        }
        }
        
    }
}