pipeline {
    agent any

    stages {
        stage('Checkout from GitHub (Test)') {
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
                stage('Confirmation test server') {
            steps {
                input(id: 'confirmDeployment', message: 'Review the test environment. If everything looks good, approve for Development.', ok: 'Deploy')
            }
        }


        stage('Deploy to main Server') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/multibranchpipeline_test/index.html student@192.168.29.64:/var/www/html/'
            }
        }
    }
}
