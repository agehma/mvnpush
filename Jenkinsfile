pipeline
{
    agent any
    stages
    {
        stage('ContinousDownload')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/dtamukong/mavenp.git'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'unable to download  the code.', cc: '', from: '', replyTo: '', subject: 'Download Failed', to: 'git.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
         stage('ContinousBuild')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh 'mvn package'
                    }
                    catch(Exception e2)
                    {
                        mail bcc: '', body: 'unable to create an artifact.', cc: '', from: '', replyTo: '', subject: 'Build Failed', to: 'developers.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
         stage('ContinousDeployment')
        {
            steps
            {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: '45d9bd40-fd6d-4ce6-9fea-586d42ba0b31', path: '', url: 'http://172.31.13.149:8080')], contextPath: 'testapp', war: '**/*.war'
                    }
                    catch(Exception e3)
                    {
                        mail bcc: '', body: 'unable to deploy the artifact into tomcat on the QA server', cc: '', from: '', replyTo: '', subject: 'Deployment Failed', to: 'middleware.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinousTesting')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/dtamukong/FunctionalTestingProf.git'
                        sh 'java -jar /var/lib/jenkins/workspace/PipelineDJ/testing.jar'
                    }
                    catch(Exception e4)
                    {
                        mail bcc: '', body: 'Selenium test scripts fail', cc: '', from: '', replyTo: '', subject: 'Testing Fail', to: 'middleware.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinousDelivery')
        {
            steps
            {
                script
                {
                    try
                    {
                        input message: 'Waiting for DM approver ', submitter: 'DJ'
                        deploy adapters: [tomcat9(credentialsId: '45d9bd40-fd6d-4ce6-9fea-586d42ba0b31', path: '', url: 'http://172.31.0.76:8080')], contextPath: 'prodapp', war: '**/*.war'
                    }
                    catch(Exception e4)
                    {
                        mail bcc: '', body: 'Unable to deliver', cc: '', from: '', replyTo: '', subject: 'Delivery Failed', to: 'delivery.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
    }
}
