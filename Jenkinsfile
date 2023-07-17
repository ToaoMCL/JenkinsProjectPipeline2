pipeline {
    agent any
    parameters {
        booleanParam(name: 'Refresh',
                    defaultValue: false,
                    description: 'Read Jenkinsfile and exit.')
		    }
    stages {
        
        stage('Integration Tests') {
            steps {
                sh '''
                      python3 -m pytest ./prime/tests/test_unit.py
                   '''
            }
        }
        stage('docker prune') {
            steps {
                sh 'docker system prune -a -f'
            }
        }

        stage('docker compose') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('connect via ssh deploy server') {
            steps {
                sh '''
                   #!/bin/bash
                   ssh -i /home/jenkins/eu-west-1-personal.pem -o StrictHostKeyChecking=no ubuntu@54.78.97.167 << EOF
		   git clone https://github.com/ToaoMCL/JenkinsProjectPipeline2
                   docker-compose -f /home/ubuntu/JenkinsProjectPipeline2/docker-compose.yaml down
                   docker system prune -af
                   docker-compose -f /home/ubuntu/JenkinsProjectPipeline2/docker-compose.yaml up -d
                   sudo rm -R /home/ubuntu/JenkinsProjectPipeline2
                   << EOF
                '''
            }
        }
        
    }
}
