pipeline {
    agent any
    
    stages {
        stage('Checkout using tag') {
            steps{
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: "refs/tags/${VERSION}"]], 
                    doGenerateSubmoduleConfigurations: false, 
                    extensions: [], 
                    submoduleCfg: [], 
                    userRemoteConfigs: [[
                        url: 'git@github.com:locngodev6205/QuickShow.git',
                        credentialsId: 'github-key-20251111'
                    ]]
                ])
            }
        }

        stage('Build & Upload Backend') {
            when {
                expression {
                    def action = env.ACTION
                    return (action == 'OnlyBE' || action == 'BOTH')
                }
            }
            steps {
                withAWS(credentials: 'pbl4-credential', region: 'ap-southeast-1') {
                    sh '''
                        # Build image backend
                        docker build -t pbl4-quickshow-backend:${VERSION} server

                        # Login ECR
                        aws ecr get-login-password --region ap-southeast-1 \
                          | docker login --username AWS --password-stdin 120915930136.dkr.ecr.ap-southeast-1.amazonaws.com

                        # Tag & push lên repo BACKEND riêng
                        docker tag pbl4-quickshow-backend:${VERSION} \
                          120915930136.dkr.ecr.ap-southeast-1.amazonaws.com/pbl4-quickshow-backend:${VERSION}

                        docker push 120915930136.dkr.ecr.ap-southeast-1.amazonaws.com/pbl4-quickshow-backend:${VERSION}
                    '''
                }
            }
        }

        stage('Build & Upload Frontend') {
            when {
                expression {
                    def action = env.ACTION
                    return (action == 'OnlyFE' || action == 'BOTH')
                }
            }
            steps {
                withAWS(credentials: 'pbl4-credential', region: 'ap-southeast-1') {
                    sh '''
                        # Build image frontend
                        docker build -t pbl4-quickshow-frontend:${VERSION} client

                        # Login ECR
                        aws ecr get-login-password --region ap-southeast-1 \
                          | docker login --username AWS --password-stdin 120915930136.dkr.ecr.ap-southeast-1.amazonaws.com

                        # Tag & push lên repo FRONTEND riêng
                        docker tag pbl4-quickshow-frontend:${VERSION} \
                          120915930136.dkr.ecr.ap-southeast-1.amazonaws.com/pbl4-quickshow-frontend:${VERSION}

                        docker push 120915930136.dkr.ecr.ap-southeast-1.amazonaws.com/pbl4-quickshow-frontend:${VERSION}
                    '''
                }
            }
        }
    }
}
