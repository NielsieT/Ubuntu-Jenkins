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
        stage('Deploy testsrv') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/multibranchpipeline_test/index.html student@192.168.29.63:/var/www/html/'
            }
        }
                stage('accept test server') {
            steps {
                input(id: 'acceptDeployment', message: 'Check test server.', ok: 'Accept')
            }
        }
        stage('Deploy Productiesrv') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/multibranchpipeline_test/index.html student@192.168.29.64:/var/www/html/'
            }
        }
    }
}
