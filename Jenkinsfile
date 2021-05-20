pipeline{	
		 agent { label 'master' }
	
		 stages
				{
					stage("Git Checkout")
						{
						 steps
								{
								 git branch: 'master', url: 'https://github.com/prashanth-konakala-bluepal/WebApp.git'
								}
						}
					stage("Maven Build")
						{
						 steps
								{
								 sh "mvn clean package"
								 sh "mv /var/lib/jenkins/workspace/WebApp/webapp/target/*.war /var/lib/jenkins/workspace/WebApp/webapp/target/webapp_main.war"
								}
						}		
					stage("Deploying to EC2")
						{
						 steps
								{
								 sshagent(['Tomcat_server-1'])
										{
										 sh """
										
											scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/WebApp/webapp/target/webapp_main.war ubuntu@18.220.148.93:/opt/tomcat/webapps/
										
										"""
										}
								}
						}
					stage("Sonarqube Analysis")
						{
						 steps
								{
									script
											{
												def scannerHome = tool 'sonarqube';
												withSonarQubeEnv("Sonarqube_Jenkinsfile")
												{
													// sh 'mvn -f pom.xml'
													sh " mvn sonar:sonar \
														-Dsonar.projectName=WebApp \
														-Dsonar.projectKey=WebApp \
														-Dsonar.host.url=http://13.234.115.101:9000/sonar \
														-Dsonar.login=90ac62d4db0cf7360246c13ca40fa8e098d9937f "
												}
											}
								}
						}	
					stage("Code Coverage")
						{
						 steps
								{
									jacoco()
								}
						}
					stage("Quality Status Check")
						timeout (time: 1, unit: 'HOURS')
								{
									def qg = waitForQualityGate()
									if (qg.status != 'OK')
										{
											error "Pipeline aborted due to Quality Gate Failure: ${qg.status}"
										}
								}
					stage ("Email Notification")
						{
							steps
									{
										mail bcc: '', body: '''Hi Welcome to Jenkins Email Alerts
										Thanks
										Prashanth''', cc:'', from: '', replyTo:'', subject: 'Jenkins Job', to: 'prashanth.konakala@bluepal.com'
									}
						}
				}	
		}
