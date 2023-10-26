pipeline {
    agent any

    stages {
        stage('GitHub Config') {
            steps {
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'test']],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CleanBeforeCheckout'],
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/NielsieT/Ubuntu-Jenkins.git']]
                    ])
                }
            }
        }
        stage('Deploy Testsrv') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/multibranchpipeline_test/index.html student@192.168.29.63:/var/www/html/'
            }
        }
                stage('Accept Testsrv') {
            steps {
                input(id: 'acceptDeployment', message: 'Check test server', ok: 'Accept')
            }
        }
        stage('Deploy Productiesrv') {
            steps {
                sh 'sshpass -p student scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/multibranchpipeline_test/index.html student@192.168.29.64:/var/www/html/'
            }
        }
    }
}
