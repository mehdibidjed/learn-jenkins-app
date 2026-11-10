pipeline{
    agent any
    environment {
        NETLIFY_SITE_ID='2f33b45d-269d-4576-a4d1-56dc59851943'
        NETLIFY_AUTH_TOKEN=credentials('netify-token')
    }
    stages{
        stage('Build'){
            agent{
                docker{
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm install
                    npm run build 
                    ls -la

                '''
            }
        }
        stage('Test'){
            agent{
                docker{
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    test -f build/index.html
                    npm test

                '''
            }
        }
        stage('Deploy'){
            agent{
                docker{
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install netlify-cli 
                    node_modules/.bin/netlify --version
                    echo "Deploying to production , Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status 
                    node_modules/.bin/netlify deploy --dir=build --prod

                '''
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }

    }
}