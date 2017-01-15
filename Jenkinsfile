node(NODE_LABEL) {
    docker.withRegistry(DOCKER_REGISTRY_URL, DOCKER_REGISTRY_CREDENTIALS_ID) {
        git url: GIT_URL, credentialsId: GIT_CREDENTIALS_ID, branch: GIT_BRANCH

        stage "build"
        def app = docker.build DOCKER_IMAGE

        stage "publish"
        app.push DOCKER_IMAGE_TAG
    }
}
