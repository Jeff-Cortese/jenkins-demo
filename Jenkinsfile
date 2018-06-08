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

                stage('Deploy GH Pages') { ->
                    sh "npm run deploy"
                }
            }

        }

        mainBuild()
    } catch(ex) {
        println ex
        throw ex;
    }
}