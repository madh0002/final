properties([pipelineTriggers([githubPush()])])
node('linux') {
    stage("Test Stack") {
       //access private git repo
       git 'https://github.com/madh0002/final.git'
        //NOTE: You may need to wrap all the following aws commands with AWS credential
        //create a cloudformation stack using a modified docker-single-server.json. Set the KeyName to the one you used to ssh your ec2 instances.
        //YourIp should be Jenkins slave IP. You can use curl ifconfig.me to get its public ip, not recommended in production though. 
sh 'aws cloudformation create-stack --stack-name final-test1 --template-body file://docker-single-server.json --region=us-east-1 --parameters ParameterKey=YourIp,ParameterValue=$(curl ifconfig.me/ip) |/32'
       sh 'curl ifconfig.me/ip'
        //NOTE: The modified json file should install redis-tools using the UserData section. docker-swarm.json has good examples. 
        //Check that docker-swarm.json. 
        //wait for the stack-create-compete
        //describe the final-test stack
        //test webhook
       sh 'aws cloudformation describe-stacks --stack-name final-test1 --region us-east-1'
        //You need to wrap all the following SSH commands with ssh agent
        //run the uptime command on docker1 over ssh.
        //You need to parse the output of the stack creation to find docker1's ip.
        //Check the link I posted on slack to find out how to parse json output using --query
        //You can also use jq to parse the output as we discussed in class      
        //I notice that not many of you know how to execute commands over ssh. So, here is an example to execute uptime command
        //ssh -o StrictHostKeyChecking=no ubuntu@<replace this with your docker1 ip> uptime
        //NOTE: The jenkins slave ip and docker1 ip should NOT be hardcoded.  
       sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
        // some block test
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@100.25.181.210 uptime'           
       }
    }
    stage("Deploy Redis") {
       sh 'docker ps -a -q'
       sh 'docker stop $(docker ps -a -q --filter ancestor=redis)'     
       sh 'docker rm $(docker ps -a -q --filter ancestor=redis)'             
       sh 'docker run --name redisimage -d redis:latest -h 100.25.181.210 -p 6379:6379'
    }
    stage("Test Redis") {
       sh 'docker ps -a -q'        
       //sshagent(['8d1f2576-2d78-4aa7-9782-8e8911d38127']) {
       //sh 'ssh ubuntu@100.25.181.210 \' exec redis-cli -h 100.25.181.210 set hello world\''
       //sh 'exec redis-cli -h 100.25.181.210 set hello world'
       //sh 'exec redis-cli set hello world'         
       //sh 'exec redis-cli get hello'     
       //test from a diff network
       }
//    }
    stage("Delete Stack") {
       sh 'docker ps -a'
    }
}
