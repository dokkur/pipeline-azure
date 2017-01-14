node(NODE_LABEL) {
    docker.withRegistry(DOCKER_REGISTRY_URL, DOCKER_REGISTRY_CREDENTIALS_ID) {
        git url: GIT_URL, credentialsId: GIT_CREDENTIALS_ID

        sh "git rev-parse HEAD > .git/commit-id"
        def commit_id = readFile('.git/commit-id').trim()
        println commit_id

        stage "build"
        def app = docker.build DOCKER_IMAGE

        stage "publish"
        app.push 'master'
        app.push "${commit_id}"
    }
}
