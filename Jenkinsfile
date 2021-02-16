pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                script{
                    
                    withAWS(region:'us-east-1',credentials:'aws_admin') {
                        haproxyip = sh (
                            script:"aws ec2 describe-instances --filters 'Name=tag:Name,Values=HAProxy' --query 'Reservations[].Instances[].PublicIpAddress[]' --output text",
                            returnStdout: true,
                        //   returnStdout: true, script: "git log --
                        //      format='%h' --no-merges -n 1" ).trim()

                        ).trim()
                        echo haproxyip
                    }

                    sshagent(credentials:['aws_ssh']){
                        sh "ssh ec2-user@${haproxyip} id"
                    }
                }
            }   
        }
    }
}
