pipeline {
    agent any

    stages {
        stage('Checkout from GitHub (Test)') {
            steps {
                // Check out your code from the "test" branch on GitHub.
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'test']],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                            [$class: 'CleanBeforeCheckout'],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/Timodo22/pijplijn.git']]
                    ])
                }
            }
        }

        stage('Deploy to Dev Server') {
            steps {
                // Copy HTML files from the "test" branch to the test server.
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Multipipeline_test/index.html student@192.168.1.18:/var/www/html/'
            }
        }

        stage('Confirmation Dev') {
            steps {
                // Prompt for confirmation before proceeding.
                input(id: 'confirmDeployment', message: 'Review the test environment. If everything looks good, approve for Development.', ok: 'Deploy')
            }
        }


        stage('Deploy to test') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Multipipeline_test/index.html student@192.168.1.23:/var/www/html/'
            }
        }
                stage('Confirmation test server') {
            steps {
                // Prompt for confirmation before proceeding.
                input(id: 'confirmDeployment', message: 'Review the test environment. If everything looks good, approve for Development.', ok: 'Deploy')
            }
        }


        stage('Deploy to main Server') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Multipipeline_test/index.html student@192.168.1.25:/var/www/html/'
            }
        }
    }
}
