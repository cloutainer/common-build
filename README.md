### :bangbang: DEPRECATED AND ARCHIVED

# common-build

Build and push images to DockerHub since DockerHub builds are slow.

Put this into a Jenkins Piepline Job

```
node {
  wrap([$class: 'HideSecretEnvVarsBuildWrapper']) {
    if (env.GWBT_REPO_FULL_NAME && env.GWBT_BRANCH != 'master' && env.GWBT_BRANCH != 'gh-pages' && env.GWBT_COMMIT_AFTER != "0000000000000000000000000000000000000000") {
        sh 'curl -H "Authorization: token ${SECRET_GITHUB_AUTH_TOKEN}" -H "Accept: application/vnd.github.v3.raw" -o Jenkinsfile -L https://api.github.com/repos/cloutainer/common-build/contents/Jenkinsfile'
        load('./Jenkinsfile')
    } else {
        echo "manual starts not allowed!"
    }
  }
}
```
 
And add the Webhook to the repo pointing to Jenkins

 * https://github.com/codeclou/jenkins-github-webhook-build-trigger-plugin
