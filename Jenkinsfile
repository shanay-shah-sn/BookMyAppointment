pipeline {
    environment {
        buildArtifactsPath = "build_artifacts/${currentBuild.number}"
        validationResultsPath = ""
    }
    agent any

    stages {
        // Build and publish application image
        stage('Build') {      
            steps {
                checkout scm    
                echo "scm checkout successful"
                
                script {
                    //dockerImageTag = env.BUILD_NUMBER
                    dockerImageName = "BookMyAppointment/web-app"
                    buildNumberArtifact = "grefId123"
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
        stage('TEST') {    
            parallel {
                stage('Code') {
                    stages('Code Steps') {
                        // Validate application code changes (SIMULATED)
                        stage('jUnit Test'){ 
                            steps {
                                echo "Running unit tests..."
                            }
                        }
                        
                        stage('Selenium Test') { 
                            steps {
                                echo "Selenium API..2..3..4"
                                sleep(time:5,unit:"SECONDS")
                                
                                echo "Selenium UI..2..3..4"
                            }
                        }
                    }                    
                }

                stage('Config') {
                    stages('Config Steps') {
                        stage('Upload') {
                            steps {
                                script {
                                    /* DevOps Config related informations */
                                    appName = 'TestMyApp'

                                    // Upload to Production US env
                                    changeSetId = snDevOpsConfigUpload(
                                         applicationName: "${appName}",
                                         target: 'deployable',
                                         deployableName: 'Production_US_1',
                                         namePath: 'helm_charts',
                                         configFile: 'k8s/helm/envs/prod_us_east/*',
                                         dataFormat: 'yaml',
                                    )

                                    // Upload to Production EU env
                                    // Commit the changeset
                                    snDevOpsConfigUpload(
                                         applicationName: "${appName}",
                                         target: 'deployable',
                                         deployableName: 'Production_EU_2',
                                         namePath: 'helm_charts',
                                         configFile: 'k8s/helm/envs/prod_eu_central/*',
                                         dataFormat: 'yaml',
                                         autoCommit: 'true',
                                         changesetNumber: "${changeSetId}"
                                    )

                                    if (changeSetId == null) {
                                        error "Changeset is not created"
                                    }
                                    
                                    echo "changeSet: $changeSetId created"
                                    
                                }
                            }
                        }

                        stage('Validate') {
                            steps {
                                script {
                                    snDevOpsConfigValidate(
                                         applicationName: "${appName}",
                                         deployableName: 'Production_US_1',
                                         markFailed: 'true',
                                         showResults: 'true'
                                    )
                                }
                            }
                        }
                    }
                }
            }
        }
        
    

        stage ('Deploy') {
            // Deploy application code and configuration data to production 1 environment
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
            
                // Deploy application code and configuration data to production 2 environment
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
