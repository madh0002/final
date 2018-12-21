properties([pipelineTriggers([githubPush()])])
node('linux') {
    parameters {
    string(name: 'dockerip',
           defaultValue: '0.0.0.0',
           description: 'Holds docker1 public IP')
    }
    stage("Test Stack") {
       //access private git repo
       git 'https://github.com/madh0002/final.git' 
       //sh 'aws cloudformation create-stack --stack-name final-test --template-body file://docker-single-server.json --region=us-east-1 --parameters ParameterKey=KeyName,ParameterValue=http://s3-us-east-1.amazonaws.com/madhu-assignment10-bucket/EastVirginaNov.pem --parameters ParameterKey=YourIp,ParameterValue=$(curl ifconfig.me/ip)/32'
       //sh 'aws cloudformation wait stack-create-complete --stack-name final-test --region us-east-1'
       sh 'aws cloudformation describe-stacks --stack-name final-test --region us-east-1' 
       script {
         dockerip = sh 'aws ec2 describe-instances --region us-east-1 --filters "Name=image-id,Values=ami-f92ff686" --query "Reservations[*].Instances[*].PublicIpAddress" --output=text'
         }
       sh 'echo "$(dockerip)"'    
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
        // Check for uptime
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.239.255.153 uptime'
       }
    }
    stage("Deploy Redis") {
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
           sh 'docker ps -a'
           sh 'ssh ubuntu@34.239.255.153 \' docker stop $(docker ps -a -q --filter ancestor=redis)\''     
           sh 'ssh ubuntu@34.239.255.153 \' docker rm $(docker ps -a -q --filter ancestor=redis)\''             
           sh 'ssh ubuntu@34.239.255.153 \' docker run --name redisimage -d redis:latest -p 6379:6379 \''
           }
       }
    stage("Test Redis") {
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
           sh 'docker ps -a'           
       //sh 'ssh ubuntu@52.90.213.249 \'docker ps -a\''
       //sh 'ssh ubuntu@34.224.70.117 \' sudo apt-get install redis-tools -y \''
       //sh 'ssh ubuntu@34.224.70.117 \' sudo apt-get install redis-server -y \''
       sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.239.255.153 \' exec redis-cli set hello world\''
       sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.239.255.153 \' exec redis-cli get hello\''
           //sh 'ssh exec redis-cli -h 34.224.70.117 set hello world'
       //sh 'ssh redis-cli set hello world'         
       //sh 'exec redis-cli get hello'     
       //test from a diff network
       }
    }
    stage("Delete Stack") {
       sh 'docker ps -a'
       //sh 'aws cloudformation delete-stack --stack-name final-test --region us-east-1'
    }
}
