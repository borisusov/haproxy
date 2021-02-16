pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                script{
                    
                    withAWS(region:'us-east-1',credentials:'aws_admin') {
                        haproxyip = sh (
                            script:"aws ec2 describe-instances --filters 'Name=tag:Name,Values=Haproxy' --query 'Reservations[].Instances[].PublicIpAddress[]' --output text",
                            returnStdout: true,
                        // returnStdout: true, script: "git log -- format='%h' --no-merges -n 1" ).trim()

                        ).trim()
                        echo haproxyip 
                    }

                    sshagent(credentials:['aws_ssh']){
                        //sh "ssh ec2-user@54.145.237.219"
                      sh """
                            scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null haproxy.cfg ec2-user@${haproxyip}:~
                      
                    """ 
                    }
                }
            }   
        }
    }
}
