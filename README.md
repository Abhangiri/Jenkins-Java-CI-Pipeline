
# Jenkins CI/CD Pipeline for Java Application

 This repository contains a Jenkins Pipeline script crafted in Groovy, offering seamless automation for the building, testing, and analysis of a Java web application. The pipeline is designed to guarantee code quality through a sequence of well-defined stages, ensuring a thorough evaluation of  Java web application's performance and reliability.


## Pipeline Overview


![App Screenshot](https://github.com/Abhangiri/Jenkins-Java-CI-Pipeline/blob/main/CI-Pipeline_flow.png?raw=true)

## Prerequisites  

Before deploying the Jenkins pipeline, ensure that the following prerequisites are met:

- **Jenkins Installation:** Ensure Jenkins is installed and configured in your environment.
- **Plugin Installation:** Install the following Jenkins plugins: `Maven Integration`, `Github Integration`, `Nexus Artifact Uploader`, `Sonarqube Scanner`, `Slack Notification`, and `Build Timestamp`.
- **Tool Configuration:** Configure Maven (MAVEN3) and JDK (OracleJDK8) in your Jenkins instance.

- **Create Nexus Repository:** Set up a Nexus repository to manage artifacts.
- **Quality Gate on Sonarqube:** Create a Quality Gate on your Sonarqube server for code analysis.
- **Jenkins Global Credentials:** Add Nexus and Sonarqube credentials under Jenkins Global Credentials.

## Configuration

- **Environment Variables:** Update the environment variables in the pipeline script `(Jenkinsfile)` to match your environment's settings. These variables include Nexus repository details, SonarQube server information, and Slack channel details.

## Usage

- **Create Jenkins Pipeline:** Set up a new Jenkins pipeline job and specify the pipeline script `(Jenkinsfile)` as the pipeline definition.

- **Run the Pipeline:** Trigger the pipeline manually or configure it to run automatically based on your needs.

## Monitoring and Review

- **Monitor Pipeline Progress:** Keep an eye on the pipeline's progress within Jenkins.

- **Review Build Output:** Examine build logs and artifacts for each stage's output.

## Notifications

Configure Slack Notifications to receive alerts and updates on your pipeline's status.