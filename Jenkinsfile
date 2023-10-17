pipeline {
    agent any

    stages {
        stage('Check GitHub') {
            steps {
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'test']],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                            [$class: 'CleanBeforeCheckout'],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/NielsieT/Ubuntu-Jenkins.git']]
                    ])
                }
            }
        }
        stage('Deploy to test') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/multibranchpipeline_test/index.html student@192.168.29.63:/var/www/html/'
            }
        }
                stage('Confirm test server') {
            steps {
                input(id: 'confirmDeployment', message: 'Check test server. approve the Development?', ok: 'Deploy')
            }
        }
        stage('Deploy to Product') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/multibranchpipeline_test/index.html student@192.168.29.64:/var/www/html/'
            }
        }
    }
}
