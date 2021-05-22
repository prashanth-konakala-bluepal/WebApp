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
										
											scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/WebApp/webapp/target/webapp_main.war ubuntu@3.129.39.83:/opt/tomcat/webapps/
										
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
												withSonarQubeEnv("Sonarqube_Pipeline")
												{
													//sh 'mvn clean install -f pom.xml'
													sh " mvn -f pom.xml sonar:sonar \
														-Dsonar.projectName=WebApp \
														-Dsonar.projectKey=WebApp \
														-Dsonar.login=b86e54949ae3cb448a47fbd9aca44dfc24fc214d "
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
						{
						 steps
								{
									timeout (time: 3, unit: 'MINUTES')
										{
											script
													{
														def qg = waitForQualityGate()
														if (qg.status != 'OK')
															{
																print "Pipeline is Not Executed due to Quality Gate Failure: ${qg.status}"
															}
														else // (qg.status = 'OK')
															{
																print "Pipleine is Executed Successfully: ${qg.status}"
															}
													}
										}	
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
