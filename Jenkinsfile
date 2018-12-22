properties([pipelineTriggers([githubPush()])])
node('linux') {
    stage("Test Variables") {
       //access private git repo
       git 'https://github.com/madh0002/final.git' 
       // script {
         //dockerip='`aws ec2 describe-instances --region us-east-1 --filters "Name=image-id,Values=ami-f92ff686" --query "Reservations[*].Instances[*].PublicIpAddress" `'
       sh 'def dockip'
       sh 'dockip="34.239.255.153"'
       //}
       sh 'echo "$dockip"'    
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
        // Check for uptime
           sh 'ssh  -o StrictHostKeyChecking=no ubuntu@\'echo "${dockip}" \' uptime'
       }
    }
}
