properties([pipelineTriggers([githubPush()])])
node('linux') {
    stage("Test Stack") {
       git 'https://github.com/madh0002/final.git'
       sh 'aws cloudinformation describe-stacks final'
    }
    stage("Deploy Redis") {
       sh 'docker ps -a'
    }
    stage("Test Redis") {
       sh 'docker ps -a'
    }
    stage("Delete Stack") {
       sh 'docker ps -a'
    }
}
