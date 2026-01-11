pipeline {
    agent any
    tools{
        nodejs 'mynode'
    }
    stages {
        stage('GitHub repo clone') {
            steps {
                echo 'cloning the github repo'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/dnyanesh-cloud/jenkins-nodeapp.git']])
                echo 'cloning repository successfully'
            }
        }
         stage('build') {
            steps {
                echo 'building the repo'
                sh 'npm install'
                echo 'build successfully'
            }
        }
        stage('test') {
            steps {
                echo 'testing the repo'
                sh './node_modules/mocha/bin/_mocha --exit ./test/test.js'
                echo 'test successsfully'
            }
        }
        stage('deploy') {
            steps {
                echo 'deploying the repo'
                sshagent(['ssh-credential-for-leveserver']) {
                    sh'''
                    ssh -o StrictHostKeyChecking=no ubuntu@65.0.131.87 << EOF
                    sudo apt update -y
                    sleep 10
                    sudo apt install nodejs -y
                    sudo apt install npm -y
                    mkdir nodeapp
                    cd nodeapp/
                    git init
                    git pull https://github.com/dnyanesh-cloud/jenkins-nodeapp.git
                    npm install
                    sudo npm install -g pm2
                    pm2 start app.js || pm2 restart app.js
                    exit
                    EOF
                    '''
                    echo 'deploy successfully'
                }
            }
        }
    }
}
