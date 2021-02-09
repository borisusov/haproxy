pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                script{
                    
                    withAWS(region:'eu-west-1',credentials:'aws_admin') {
                        haproxyip = sh (
                            script:"aws ec2 describe-instances --filters 'Name=tag:Name,Values=HAProxy' --query 'Reservations[].Instances[].PublicIpAddress[]' --output text",
                            returnStdout: true,
                        )
                        echo haproxyip
                    }
                }
            }
        }
    }
}
