pipeline
{
    agent any
    stages
    {
        stage('continuousDownload')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/dtamukong/mavenp'
                    }
                    catch (Exception e1)
                    {
                        mail bcc: '', body: 'git unable to download code from github repository', cc: '', from: '', replyTo: '', subject: 'Git failed to download', to: 'gitteam@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousBuild')
        {
            steps
            {
                script
                {
                    try
                    {
                       sh 'mvn package' 
                    }
                    catch (Exception e2)
                    {
                       mail bcc: '', body: 'mvn failed to build an artifact', cc: '', from: '', replyTo: '', subject: 'mvn failed to build', to: 'devteam@gmail.com'
                       exit(1)
                    }
                }
            }
        }
        stage('ContinuousDeployment')
        {
            steps
            {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: '3933de1f-781a-4384-bab7-5258db7fbfd3', path: '', url: 'http://172.31.85.178:8080')], contextPath: 'testapp', war: '**/*.war'
                    }
                    catch (Exception e3)
                    {
                        mail bcc: '', body: 'artifacts not deployed into qa server ', cc: '', from: '', replyTo: '', subject: 'failed to deploy to qa container', to: 'middlewareteam@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousTesting')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/dtamukong/FunctionalTestingProf.git'
                        sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipeline-Exception/testing.jar'
                    }
                    catch (Exception e4)
                    {
                        mail bcc: '', body: 'selenium script failed to test artifacts', cc: '', from: '', replyTo: '', subject: 'selenium failed', to: 'qateam@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousDelivery')
        {
            steps
            {
                script
                {
                    try
                    {
                        input message: 'DM to approve', submitter: 'jaden'
                        deploy adapters: [tomcat9(credentialsId: '3933de1f-781a-4384-bab7-5258db7fbfd3', path: '', url: 'http://172.31.85.56:8080')], contextPath: 'prodapp', war: '**/*.war'
                    }
                    catch (Exception e5)
                    {
                        mail bcc: '', body: 'unable to deliver into tomcat prodserver', cc: '', from: '', replyTo: '', subject: 'delivery failed', to: 'deliveryteam@gmail.com'
                    }
                }
            }
        }
    }
}
