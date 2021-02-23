pipeline {
    agent any

    stages {
        stage('Deploy') {
            steps {
                script{
                    
                    withAWS(region:'us-east-1',credentials:'aws_admin') {
                        haproxyip = sh (
                            script:"aws ec2 describe-instances --filters 'Name=tag:Name,Values=Haproxy' --query 'Reservations[].Instances[].PublicIpAddress[]' --output text",
                            returnStdout: true,      
                        ).trim()
                        echo haproxyip 
                    }

                    sshagent(credentials:['aws_ssh']){
                        
                      sh """
                            scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null haproxy.cfg ec2-user@${haproxyip}:~                            
                    """ 
                    }

                    // sshagent(credentials:['aws_ssh']){
                        
                    //   sh """                            
                    //         ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ec2-user@${haproxyip} /usr/sbin/haproxy -c -V -f haproxy.cfg
                    // """ 
                    // } 
                }                
            } 
        }
      
       stage('Validation') {
            steps {
                script{
                    
                    withAWS(region:'us-east-1',credentials:'aws_admin') {
                        haproxyip = sh (
                            script:"aws ec2 describe-instances --filters 'Name=tag:Name,Values=Haproxy' --query 'Reservations[].Instances[].PublicIpAddress[]' --output text",
                            returnStdout: true,      
                        ).trim()
                        echo haproxyip 
                    }

                    

                    sshagent(credentials:['aws_ssh']){
                        
                      sh """                            
                            ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ec2-user@${haproxyip} /usr/sbin/haproxy -c -V -f haproxy.cfg

                    """ 
                    } 
                }                
            } 
        }

      stage('Additional') {
            steps {
                script{
                    
                    withAWS(region:'us-east-1',credentials:'aws_admin') {
                        haproxyip = sh (
                            script:"aws ec2 describe-instances --filters 'Name=tag:Name,Values=Haproxy' --query 'Reservations[].Instances[].PublicIpAddress[]' --output text",
                            returnStdout: true,      
                        ).trim()
                        echo haproxyip 
                    }

                    sshagent(credentials:['aws_ssh']){
                        
                      sh """
                            echo "#!/bin/bash" > deploy.sh
                            echo "mv haproxy.cfg /etc/haproxy/haproxy.cfg" >> deploy.sh
                            echo "systemctl reload haproxy" >> deploy.sh
                            chmod +x deploy.sh
                            

                            scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null deploy.sh ec2-user@${haproxyip}:~
                            

                            ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ec2-user@${haproxyip} sudo ./deploy.sh                
                                  
                     
                    """ 
                    }

                    
                }                
            } 
        }





    }
}
