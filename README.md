## Overview

This repository is an example of setting up the SonarScanner Analysis with GitHub Actions pipeline for a java/gradle project.  
We have multiple CI/CD Pipeline examples, one for connecting to SonarQube Server instance and the other to SonarQube Cloud instance.   

__**PLEASE READ OUR SONARQUBE DOCUMENTATION FOR WORKING WITH GITHUB ACTIONS PIPELINES**__  
[GitHub Actions Workflow with SonarQube Scanner](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/github-integration/adding-analysis-to-github-actions-workflow/)  

## Important Information in Pipelines
- On triggers are set to only execute on changes to specific branches and a specific directory in the project, this can be modified with whatever trigger you would want to use.
- They have shallow fetch set to 0. this is required for SonarScanner to properly analyze your project.  
- For more information on how to limit your analysis scope and parameters available, please check **SonarScanner Analysis Scope** and **SonarScanner Analysis Parameters** in the Important Links section.
- The action used for SonarScanner Analysis is execute via the Gradle command, which applies for both SonarQube Server and SonarQube Cloud. But they require different parameters. Examples for both are provided.
    - SonarQube Cloud Example: sonarqube-cloud.yml  
    - SonarQube Server Example: sonarqube-server.yml 

## Important Links
[SonarQube Server - GitHub Integration](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/github-integration/introduction/)  
[SonarQube Cloud - GitHub Integration](https://docs.sonarsource.com/sonarqube-cloud/getting-started/github/)  
[GitHub Actions Workflow with SonarQube Scanner](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/github-integration/adding-analysis-to-github-actions-workflow/)  
[SonarScanner Analysis Scope](https://docs.sonarsource.com/sonarqube-server/latest/project-administration/analysis-scope/)  
[SonarScanner Analysis Parameters](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/analysis-parameters/)  
[SonarScanner for Gradle](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/scanners/sonarscanner-for-gradle/)  

## Example to fail the entire pipeline if Quality Gate fails
There may be situations or branches in which you will like to fail the pipeline if the SonarQube Quality Gate fails in order to stop any other steps in the pipeline.  
This can be done by adding `sonar.qualitygate.wait=true` 
to the parameters section in the **Gradle** command. 

Example:
``` sh
    ./gradlew build sonar --info -d \
          -Dsonar.projectKey=$(echo $GITHUB_REPOSITORY | cut -d'/' -f1)-gh_$(echo $GITHUB_REPOSITORY | cut -d'/' -f2) \
          -Dsonar.projectName=$(echo $GITHUB_REPOSITORY | cut -d'/' -f1)-gh_$(echo $GITHUB_REPOSITORY | cut -d'/' -f2) \
          -Dsonar.qualitygate.wait=true
```