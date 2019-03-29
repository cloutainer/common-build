sh 'curl -sSLko pipeline-helper.groovy ${K8S_INFRASTRUCTURE_BASE_URL}pipeline-helper/pipeline-helper.groovy?v2'
def pipelineHelper = load("./pipeline-helper.groovy")
pipelineHelper.jenkinsDiscardOldBuilds(10)
pipelineHelper.jenkinsDisableConcurrentBuilds()
pipelineHelper.deployTemplate {
    stage('clone') {
        sh 'rm -rf source | true'
        sh 'git clone --single-branch --branch $GWBT_BRANCH$GWBT_TAG https://${GITHUB_AUTH_TOKEN}@github.com/${GWBT_REPO_FULL_NAME}.git source'
        dir('source') {
            sh 'git reset --hard $GWBT_COMMIT_AFTER'
        }
    }
    stage('build & push docker image') {
      dir('source') {
        // LOGINTO DOCKERUB
        sh 'ls -la'
        sh 'docker login -u cloutainer -p ${DOCKERHUB_CLOUTAINER_PASSWORD}'
        sh 'docker build . -t cloutainer/' + env.GWBT_REPO_NAME + ':' + env.GWBT_BRANCH + env.GWBT_TAG
        sh 'docker push cloutainer/' + env.GWBT_REPO_NAME + ':' + env.GWBT_BRANCH + env.GWBT_TAG
      }
    }
}
