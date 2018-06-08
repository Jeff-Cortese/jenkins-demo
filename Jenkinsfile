#!groovy

node('crowsnest') {
    checkout scm
    echo "Branch name ${env.BRANCH_NAME}"

    try {
        def nodeImage = docker.image("node:8.11.2")
        nodeImage.inside { ->
            stage("Build") { ->
                sh "npm install"
                sh "npm run build"
            }

            stage("Test") { ->
                withEnv(["CI=true"]) { ->
                    sh "npm test"
                }
            }
        }

        def awsCli = docker.image("cleo/crows-awscli:0.0.1")
        awsCli.inside { ->
            stage('Deploy') {
                def s3BucketName = "jcortese-jenkins-demo"
                sh "aws s3 sync ./build s3://${s3BucketName} --acl \"public-read\""
                sh "aws s3 website s3://${s3BucketName} --index-document index.html --error-document index.html" // host the bucket as a web site"
            }
        }

        slackSend channel: "@jeff", color: "good", message: "http://jcortese-jenkins-demo.s3-website-us-west-2.amazonaws.com/"
    } catch(ex) {
        slackSend channel: "@jeff", color: "danger", message: ex.message
        throw ex
    }
}