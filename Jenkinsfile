properties([pipelineTriggers([githubPush()])])
node('linux') {
    environment {
        dockip="text"
    }
    stage("Test Variables") {
        
       //access private git repo
       git 'https://github.com/madh0002/final.git' 
       
       sh 'echo '\Madhu'\ > name.txt'
       sh 'echo name.txt'
        // script {
         //dockerip='`aws ec2 describe-instances --region us-east-1 --filters "Name=image-id,Values=ami-f92ff686" --query "Reservations[*].Instances[*].PublicIpAddress" `'
       //}
         sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
        // Check for uptime
           sh 'ssh  -o StrictHostKeyChecking=no ubuntu@\'name.txt \' uptime'
       }
    }
}
