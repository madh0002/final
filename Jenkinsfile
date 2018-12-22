properties([pipelineTriggers([githubPush()])])
def dockip;
def dockerip;
node('linux') {
    stage("Test Stack") {
       //access private git repo
       git 'https://github.com/madh0002/final.git' 
       // create stack and describe
       sh 'aws cloudformation create-stack --stack-name final-test --template-body file://docker-single-server.json --region=us-east-1 --parameters ParameterKey=KeyName,ParameterValue=http://s3-us-east-1.amazonaws.com/madhu-assignment10-bucket/EastVirginaNov.pem --parameters ParameterKey=YourIp,ParameterValue=$(curl ifconfig.me/ip)/32'
       sh 'aws cloudformation wait stack-create-complete --stack-name final-test --region us-east-1'
       sh 'aws cloudformation describe-stacks --stack-name final-test --region us-east-1' 
        // Retrieve docker1 public IP and parse
           sh """
           aws ec2 describe-instances --region us-east-1 --filters "Name=image-id,Values=ami-f92ff686" --query "Reservations[*].Instances[*].PublicIpAddress" > dockip
           cat dockip
           cat dockip | tr -d '[]",[:space:]' > dockerip
           cat dockerip
           """
        // check uptime
         sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
           sh 'ssh -o StrictHostKeyChecking=no ubuntu@$(cat dockerip) uptime'
       }
    }
    stage("Deploy Redis") {
        // Deploying redis image
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
           //sh 'ssh ubuntu@$(cat dockerip) \' docker stop $(docker ps -a -q --filter ancestor=redis)\''     
           //sh 'ssh ubuntu@$(cat dockerip) \' docker rm $(docker ps -a -q --filter ancestor=redis)\''             
           sh 'ssh ubuntu@$(cat dockerip) \' docker run --name redisimage -d redis:latest -p 6379:6379 \''
           }
       }
    stage("Test Redis") {
        // test to make sure redis is reachable
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
           sh 'docker ps -a'           
       //sh 'ssh ubuntu@52.90.213.249 \'docker ps -a\''
       //sh 'ssh ubuntu@34.224.70.117 \' sudo apt-get install redis-tools -y \''
       //sh 'ssh ubuntu@34.224.70.117 \' sudo apt-get install redis-server -y \''
       sh 'ssh -o StrictHostKeyChecking=no ubuntu@$(cat dockerip) \' exec redis-cli set hello world\''
       sh 'ssh -o StrictHostKeyChecking=no ubuntu@$(cat dockerip) \' exec redis-cli get hello\''
       }
    }
    stage("Delete Stack") {
         // delete the stack on successful test
       sh 'aws cloudformation delete-stack --stack-name final-test --region us-east-1'
    }
}
