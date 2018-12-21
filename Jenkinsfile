properties([pipelineTriggers([githubPush()])])
node('linux') {
    stage("Test Stack") {
       //access private git repo
       git 'https://github.com/madh0002/final.git'
       //sh 'aws cloudformation create-stack --stack-name final-test --template-body file://docker-single-server.json --region=us-east-1 --parameters ParameterKey=YourIp,ParameterValue=$(curl ifconfig.me/ip)/32'
       //sh 'aws cloudformation wait stack-create-complete --stack-name final-test --region us-east-1'
       sh 'aws cloudformation describe-stacks --stack-name final-test --region us-east-1' 
       sh 'aws ec2 describe-instances --region us-east-1'
       
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
        // Check for uptime
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@54.172.86.78 uptime'           
       }
    }
    stage("Deploy Redis") {
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
           //sh 'docker ps -a'
           sh 'ssh ubuntu@54.172.86.78 \' docker stop $(docker ps -a -q --filter ancestor=redis)\''     
           sh 'ssh ubuntu@54.172.86.78 \' docker rm $(docker ps -a -q --filter ancestor=redis)\''             
           sh 'ssh ubuntu@54.172.86.78 \' docker run --name redisimage -d redis:latest -h 54.172.86.78 -p 6379:6379 \''
           }
       }
    stage("Test Redis") {
       sh 'docker ps -a'        
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
       sh 'ssh ubuntu@54.172.86.78 \' sudo apt-get install redis-tools -y \''
       sh 'ssh ubuntu@54.172.86.78 \' redis-cli set hello world\''
       // sh 'ssh exec redis-cli -h 3.80.250.214 set hello world'
       //sh 'ssh redis-cli set hello world'         
       //sh 'exec redis-cli get hello'     
       //test from a diff network
       }
    }
    stage("Delete Stack") {
       sh 'docker ps -a'
       sh 'aws cloudformation delete-stack --stack-name final-test'
    }
}
