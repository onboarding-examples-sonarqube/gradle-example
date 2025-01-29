## Overview

This project is an example of using Azure DevOps for a java/gradle project. It demonstrates how to set up a CI/CD pipeline for a java/gradle application.  
We have multiple CI/CD Pipeline examples, one for running the SonarScanner and sending the results to SonarQube Server and the other for sending the results to SonarQube Cloud.  

__**PLEASE READ OUR SONARQUBE DOCUMENTATION FOR WORKING WITH GITHUB ACTIONS PIPELINES**__  
[GitHub Actions Workflow with SonarQube Scanner](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/github-integration/adding-analysis-to-github-actions-workflow/)  

## Important Information in Pipelines
- triggers are set to only execute on changes to main branch and a specific directory in the project, this can be modified with whatever you would want to specify.
- they have shallow fetch set to 0. this is required for SonarScanner to properly analyze your project.  
- please remember to check the version available for the **cliScannerVersion** (https://github.com/SonarSource/sonar-scanner-cli/tags) parameter in the **SonarQubePrepare/SonarCloudPrepare** step.
- for more information on how to limit your analysis scope and parameters available, please check **SonarScanner Analysis Scope** and **SonarScanner Analysis Parameters** in the Important Links section.
- For running the SonarScanner via Gradle, it is the same command for SonarQube Server and SonarQube Cloud. The only thing that changes are the parameters for the URL and for Cloud you will need to add the organization name. Examples for both are provided.
    - SonarQube Cloud Example: SonarQube-Cloud.yml  
    - SonarQube Server Example: SonarQube-Server.yml 

## Important Links
[SonarQube Server - GitHub Integration](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/github-integration/introduction/)  
[SonarQube Cloud - GitHub Integration](https://docs.sonarsource.com/sonarqube-cloud/getting-started/github/)  
[GitHub Actions Workflow with SonarQube Scanner](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/github-integration/adding-analysis-to-github-actions-workflow/)  
[SonarScanner Analysis Scope](https://docs.sonarsource.com/sonarqube-server/latest/project-administration/analysis-scope/)  
[SonarScanner Analysis Parameters](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/analysis-parameters/)  

## Example to fail the entire pipeline if Quality Gate fails
There may be situations or branches in which you will like to fail the pipeline if the SonarQube Quality Gate fails in order to stop any other steps in the pipeline.  
This can be done by adding ```python
sonar.qualitygate.wait=true``` 
to the **extraProperties** section in the **Maven** command.  

Example
``` sh
    ./gradlew build sonar --info -d \
          -Dsonar.projectKey=$(echo $GITHUB_REPOSITORY | cut -d'/' -f1)-gh_$(echo $GITHUB_REPOSITORY | cut -d'/' -f2) \
          -Dsonar.projectName=$(echo $GITHUB_REPOSITORY | cut -d'/' -f1)-gh_$(echo $GITHUB_REPOSITORY | cut -d'/' -f2) \
          -Dsonar.qualitygate.wait=true
```