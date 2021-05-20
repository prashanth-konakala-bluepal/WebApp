pipeline 
        {
            agent any
            stages
                {
                    stage ("Git Checkout")
                            {
                                steps 
                                        {
                                            git branch: 'master', url: 'https://github.com/prashanth-konakala-bluepal/Sonarqube_Sample_Code.git'
                                        }
                            }
                    stage ("Build")
                            {
                                steps
                                        {
                                            // sh 'mvn clean install -f MyWebApp/pom.xml'
                                            sh 'mvn clean package -f pom.xml'
                                        }
                            }
                    stage ("Sonar Analysis")
                            {
                                steps
                                        {
                                            script 
                                                    {
                                                        def scannerHome = tool 'sonarqube';
                                                        withSonarQubeEnv("Sonarqube_Jenkinsfile")
                                                        {
                                                         sh 'mvn clean install -f pom.xml'
                                                         sh " mvn -f pom.xml sonar:sonar \
                                                              -Dsonar.projectName=Sonarqube_Jenkinsfile_Pipeline \
                                                              -Dsonar.projectKey=Sonarqube_Jenkinsfile_Pipeline \
                                                              -Dsonar.login=90ac62d4db0cf7360246c13ca40fa8e098d9937f "
                                                        }
                                                    }
                                        }
                            }
                    stage ("Code Coverage")
                            {
                               steps 
                                    {
                                      jacoco()       
                                    }
                            }
                }
        }
