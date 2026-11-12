pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '6a7616a5-3dca-4bd3-9743-f4ce33ee645c'
        NETLIFY_AYTH_TOKEN =credentials('netlify-token')
    }
    
    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                ls -la
                node --version
                npm --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }
        stage('Test'){
             agent{
                docker{
                    image 'node:18-alpine'
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



         stage('Deploy') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo "Deploying to production . Site ID: NETLIFY_SITE_ID "
                node_modules/.bin/netlify status
                node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }    
    }

}
