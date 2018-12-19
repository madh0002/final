properties([pipelineTriggers([githubPush()])])
node('linux') {
    stage("Test Stack") {
       //access private git repo
       git 'https://github.com/madh0002/final.git'
        //NOTE: You may need to wrap all the following aws commands with AWS credential
        //create a cloudformation stack using a modified docker-single-server.json. Set the KeyName to the one you used to ssh your ec2 instances.
        //YourIp should be Jenkins slave IP. You can use curl ifconfig.me to get its public ip, not recommended in production though. 
       sh 'aws cloudformation create-stack --stack-name final-test --template-body file://docker-single-server.json --parameters ParameterKey=KeyPairName,ParameterValue=Finals --region=us-east-1' 
       sh 'curl ifconfig.me'
        //NOTE: The modified json file should install redis-tools using the UserData section. docker-swarm.json has good examples. 
        //Check that docker-swarm.json. 
        //wait for the stack-create-compete
        //describe the final-test stack
        //test webhook
       sh 'aws cloudformation describe-stacks --stack-name final --region us-east-1'
        //You need to wrap all the following SSH commands with ssh agent
        //run the uptime command on docker1 over ssh.
        //You need to parse the output of the stack creation to find docker1's ip.
        //Check the link I posted on slack to find out how to parse json output using --query
        //You can also use jq to parse the output as we discussed in class      
        //I notice that not many of you know how to execute commands over ssh. So, here is an example to execute uptime command
        //ssh -o StrictHostKeyChecking=no ubuntu@<replace this with your docker1 ip> uptime
        //NOTE: The jenkins slave ip and docker1 ip should NOT be hardcoded.                
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


