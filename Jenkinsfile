properties([pipelineTriggers([githubPush()])])
node('linux') {
    stage("Test Stack") {
       git 'https://github.com/madh0002/assignment11.git'
       sh 'aws s3 cp s3://madhu-assignment10-bucket/classweb.html ./index.html '
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
