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
    } catch(ex) {
        //TODO slack exeception
        throw ex
    }
}