pipeline {
    environment {
        buildArtifactsPath = "build_artifacts/${currentBuild.number}"
        validationResultsPath = ""
    }
    agent any
    /**
    * Jenkins pipline related variables
    */
    stages {
        // Initialize pipeline
        stage('Initialize') {
            steps {
                script {
                    dockerImageName = "BookMyAppointment/web-app"
                    buildNumberArtifact = "grefId123"

                }
                echo """---- Build Parameters ----
                applicationName: ${appName}
                """
            }
        }
            
        // Build and publish application image
        stage('Build') {      
            steps {
                checkout scm    
                echo "scm checkout successful"
                
                script {
                    //dockerImageTag = env.BUILD_NUMBER
                    dockerImageTag = "1.2"
                    dockerImageNameTag = "${dockerImageName}" + ":" + "${dockerImageTag}"

                    snDevopsArtifactPayload = '{"artifacts": [{"name": "' + dockerImageName + '",  "version": "' + "${dockerImageTag}" + '", "semanticVersion": "' + "0.1.${dockerImageTag}"+ '","repositoryName": "' + dockerImageName+ '"}, ],"stageName":"Build image","branchName": "main"}'  ;
                    echo "Docker image artifact: ${dockerImageNameTag} "
                    echo "snDevopsArtifactPayload: ${snDevopsArtifactPayload} "

                    snDevOpsArtifact(artifactsPayload:snDevopsArtifactPayload)
                }
            }
        } 
            
        // Validate code and config data
        stage('Validate') {    
            // Validate application code changes (SIMULATED)
            stages {
                stage('jUnit Test'){ 
                    steps {
                        echo "Running unit tests..."
                    }
                }
                
                stage('SonarQube analysis') {
                    steps {
                        echo "Running code quality analysis..."
                    }
                }
            }    
        }
        
        // Run functional tests
        stage ('Functional Testing') {
            parallel {      
                stage('Selenium API') { 
                    steps {
                        echo "Selenium API..2..3..4"
                        sleep(time:5,unit:"SECONDS")

                        echo "Selenium UI..2..3..4"
                        sleep(time:7,unit:"SECONDS")
                    }
                }
            }
        }

        // Deploy application code and configuration data to production environment
        stage('Deploy to Production') {
                steps {
                    script {
                            echo "Show exported config data from file name"
                            echo " ++++++++++++ BEGIN OF File Content ***************"
                            echo " ++++++++++++ END OF File content ***************"
                            echo "Exported config data handed off to deployment tool"
                            echo "********************** BEGIN Deployment ****************"
                            echo "Applying docker image ${dockerImageNameTag}"
                            echo "********************** END Deployment ****************"
                    }
                }
        }
    }
}
