node(NODE_LABEL) {
    docker.withRegistry(DOCKER_REGISTRY_URL, DOCKER_REGISTRY_CREDENTIALS_ID) {
        git url: GIT_URL, credentialsId: GIT_CREDENTIALS_ID, branch: GIT_BRANCH

        if (GIT_TAG) {
            sh "git checkout ${GIT_TAG}"
        }

        if (GIT_COMMIT) {
            sh "git checkout ${GIT_COMMIT}"
        }

        sh "git rev-parse HEAD > .git/commit-id"
        def commit_id = readFile('.git/commit-id').trim()
        def image_tag = commit_id

        stage "build"
        def app = docker.build DOCKER_IMAGE

        stage "publish"
        if (DOCKER_IMAGE_TAG) {
            image_tag = DOCKER_IMAGE_TAG
        }
        app.push image_tag

        sh "docker run -i -e AZURE_STORAGE_ACCOUNT=${AZURE_STORAGE_ACCOUNT} -e AZURE_STORAGE_ACCESS_KEY=${AZURE_STORAGE_ACCESS_KEY} -v \$(pwd):/project --entrypoint=python dokkur/azure-fileshare-sync -m src.swanager_sync /project/swanager.json ${AZURE_FILESHARE}/${SWANAGER_USERNAME}/${SWANAGER_APPLICATION}"
    }
}
