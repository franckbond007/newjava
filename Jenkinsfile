pipeline
{
    agent any
    stages
    {
        stage('ContinuousDownload')
        {
            steps
            {
                script
                {
                  try
                  {
                    git 'https://github.com/franckbond007/newjava.git'  
                  }
                  catch(Exception e1)
                  {
                    mail bcc: '', body: 'Jenkins is unable to download from the development code remote github', cc: '', from: '', replyTo: '', subject: 'Download failed', to: 'git.admin@gmail.com'  
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
                    catch(Exception e2)
                    {
                      mail bcc: '', body: 'Jenkins is unable to create an artifact from the code', cc: '', from: '', replyTo: '', subject: 'Build failed', to: 'dev.team@gmail.com'
                      exit(1)
                    }
                }
            }
        }    
        stage('ContinuousDeploy')
        {
            steps
            {
                script
                {
                    try
                    {
                       deploy adapters: [tomcat9(credentialsId: '64325657-48db-4197-a33f-2c10def1ce99', path: '', url: 'http://172.31.20.47:8080')], contextPath: 'test1', war: '**/*.war' 
                    }
                    catch(Exception e3)
                    {
                      mail bcc: '', body: 'Jenkins is unable to deploy into tomcat on the QAServer', cc: '', from: '', replyTo: '', subject: 'Deployment failed', to: 'middleware.team@gmail.com'
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
                      git 'https://github.com/intelliqittrainings/FunctionalTesting.git'
                      sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipeline/testing.jar' 
                    }
                    catch(Exception e4)
                    {
                        mail bcc: '', body: 'Selenium test scripts are giving a failure status', cc: '', from: '', replyTo: '', subject: 'Testing failed', to: 'qa.team@gmail.com'
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
                      deploy adapters: [tomcat9(credentialsId: '64325657-48db-4197-a33f-2c10def1ce99', path: '', url: 'http://172.31.15.28:8080')], contextPath: 'prod1', war: '**/*.war'  
                    }
                    catch(Exception e5)
                    {
                       mail bcc: '', body: 'Jenkins is unable to deploy to tomcat on the prodserver', cc: '', from: '', replyTo: '', subject: 'Delivery failed', to: 'delivery@gmail.com'
                       exit(1)
                    }
                }
                
            }
            
        }
    }
}         
