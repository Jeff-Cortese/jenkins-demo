#!groovy

node('crowsnest') {
    checkout scm

    try {
        def mainBuild = { ->
            def nodeImage = docker.image('node:8.11.2')

            nodeImage.inside { ->
                stage('build') {
                    sh "npm install"
                    sh "npm run build"
                }

                stage('Test') { ->
                    withEnv(["CI=true"]) { ->
                        sh "npm test"
                    }
                }
            }

            def awsCli = docker.image('cleo/crows-awscli:0.0.1')
            awsCli.inside { ->
                stage('Deploy') { ->
                    def s3BucketName = "jcortese-jenkins-demo"
                    sh "aws s3 sync ./build s3://${s3BucketName} --acl \"public-read\""
                    sh "aws s3 website s3://${s3BucketName} --index-document index.html --error-document index.html" // host the bucket as a web site"
                }
            }
        }

        mainBuild()
    } catch(ex) {
        println ex
        throw ex;
    }
}