# DevSecOps-AWS-Security-Automation
This project implements a DevSecOps pipeline for integrating security scanning throughout the application development lifecycle within the AWS environment. The pipeline leverages various tools to identify and address security vulnerabilities early, ensuring a more secure and robust application.
# Objectives:
 <a href="https://imgur.com/9BxctDm"><img src="https://i.imgur.com//9BxctDm.png" title="source: imgur.com" /></a>
# Implementation:
This project involves configuring CodePipeline, CodeBuild, and integrating the chosen security scanning tools (SonarCloud, Snyk, TruffleHog) with the pipeline.  Additionally, IAM roles and policies need to be set up to grant the pipeline and build services access to the CodeCommit repository and required AWS resources.
# Code Repository (AWS CodeCommit) 
This serves as the central repository where developers store their application code.
# SonarCloud (SAST) Static Application Security Testing
Performs Static Application Security Testing (SAST) by analyzing the application code directly in CodeCommit for vulnerabilities like SQL injection, insecure coding practices, and potential code smells.
# TruffleHog
Scans code for secrets like passwords, API keys, and other sensitive information that might be accidentally committed to the repository.
# Snyk (SCA)
Conducts Software Composition Analysis (SCA) by scanning dependencies within the code for known vulnerabilities. This identifies potential risks introduced by third-party libraries used in the application.
# Build and Test (AWS CodeBuild)
This service builds the application and executes the security scans mentioned above.
# Deployment Pipeline (AWS CodePipeline)
Orchestrates the entire development and security testing process.
# Upon a code commit in CodeCommit
- CodePipeline triggers a build in CodeBuild.
- CodeBuild retrieves the code, runs SonarCloud SAST, Snyk SCA, and TruffleHog scans, and integrates the results into the pipeline.
- CodePipeline evaluates the scan results. If vulnerabilities are detected, it can notify developers or prevent deployment until the issues are addressed.
# OWASP ZAP (Zed Attack Proxy) 
Zap Proxy is used as a Dynamic Application Security Testing (DAST) tool. DAST involves running the application to and analyze its behavior for vulnerabilities. This can be a valuable addition for a comprehensive security assessment.
Benefits:
# Steps
- Create a repository on AWS CodeCommit

  <a href="https://imgur.com/khwwKHm"><img src="https://i.imgur.com/khwwKHm.png" title="source: imgur.com" /></a>
 
- IAM > Users > Select user > Security credentials > HTTPS Git credentials for AWS CodeCommit > Generate credentials > Save credential

  <a href="https://imgur.com/smditP1"><img src="https://i.imgur.com/smditP1.png" title="source: imgur.com" /></a>

- Clone repository Copy URL > Git > git clone https://git-codecommit_us-east-1..........

  <a href="https://imgur.com/FwqfVua"><img src="https://i.imgur.com//FwqfVua.png" title="source: imgur.com" /></a>
 
- Add code to the folder > Git > git add * > git commit -m "Adding files"
  <a href="https://imgur.com/4DH99Wb"><img src="https://i.imgur.com/4DH99Wb.png" title="source: imgur.com" /></a>
  
- Git > git push
  
  <a href="https://imgur.com/sohPmMX"><img src="https://i.imgur.com/sohPmMX.png" title="source: imgur.com" /></a>

- Create a buildspec.yml file

  <a href="https://imgur.com/CNSwDZx"><img src="https://i.imgur.com/CNSwDZx.png" title="source: imgur.com" /></a>

- Create organization project on SonarCloud web browser for Static Application Security Testing (SAST)
- Update the buildspec.yml file with the SonalCloud project key, organization and token in AWS web browser or CLI from VSCODE
- Using Git update buildspec.yml file with, git add * > git commit -m "buildspec file updated" > git push
- Create a CodeBuild Project instance on AWS > Start build

  <a href="https://imgur.com/wC7DefZ"><img src="https://i.imgur.com/wC7DefZ.png" title="source: imgur.com" /></a>

- Observe the build scan result on SonarCloud

  <a href="https://imgur.com/WMyXsSp"><img src="https://i.imgur.com/WMyXsSp.png" title="source: imgur.com" /></a>

- Add Java files for unit test and better scan coverage

  <a href="https://imgur.com/Bz5BXjV"><img src="https://i.imgur.com/Bz5BXjV.png" title="source: imgur.com" /></a>

- Config secret key in AWS secret manager > Store a new secret > Select "other type" of secret (API token) > Add the Key-name / Value-token

  <a href="https://imgur.com/VOeOLbg"><img src="https://i.imgur.com/VOeOLbg.png" title="source: imgur.com" /></a>

- Install TruffleHog and run a scan for leaked credential

  <a href="https://imgur.com/mEg6Yj9"><img src="https://i.imgur.com/mEg6Yj9.png" title="source: imgur.com" /></a>
 
- TruffleHog scan result
- 
  <a href="https://imgur.com/jICjIcO"><img src="https://i.imgur.com/jICjIcO.png" title="source: imgur.com" /></a

- Update buildspec.yml file as follows 
    - env:
    - secrets-manager:
    - TOKEN: sonarcloud1:tokenForSonar
    - Dsonar.login=$TOKEN
  <a href="https://imgur.com/5O7fzOj"><img src="https://i.imgur.com/5O7fzOj.png" title="source: imgur.com" /></a>
  
 - Create a CI/CD Pipeline on AWS
  <a href="https://imgur.com/BJiD0x7"><img src="https://i.imgur.com/BJiD0x7.png" title="source: imgur.com" /></a>
   
 - Add a stage between Source and Build stage and connect with the Snyk Software Composition Analysis tool
  <a href="https://imgur.com/Hb4bk8p"><img src="https://i.imgur.com/Hb4bk8p.png" title="source: imgur.com" /></a>
 
 - Snyk SCA scan result

   <a href="https://imgur.com/vGACmuW"><img src="https://i.imgur.com/vGACmuW.png" title="source: imgur.com" /></a>

  - Create an Artifacts storage

   <a href="https://imgur.com/uRh3sUl"><img src="https://i.imgur.com//uRh3sUl.png" title="source: imgur.com" /></a>
  # Deploying DAST with OWASP zap (Zed Attack Proxy)
  
  - Rename old buildspec.yml file and add a new buildspec file with the following code

   <a href="https://imgur.com/s8fsn1U"><img src="https://i.imgur.com/s8fsn1U.png" title="source: imgur.com" /></a>

  - Git > git add .

   - Git > git push

   - AWS > CodePipeline > Release change for DAST OWASP ZAP (Zed Attack Proxy) scan
   - Succesful build pipeline stages
     - Source stage
     - SCA(Sofware Composition Analysis) stage
     - Build stage 
   - OWASP ZAP scan report
     
      <a href="https://imgur.com/XEHUu9z"><img src="https://i.imgur.com//XEHUu9z.png" title="source: imgur.com" /></a>

     <a href="https://imgur.com/HLTJ68z"><img src="https://i.imgur.com/HLTJ68z.png" title="source: imgur.com" /></a>
# Conclusion:

By implementing this DevSecOps pipeline, organizations can significantly improve the security posture of their applications. Integrating security scans throughout the development lifecycle fosters a "security-first" mentality and ultimately leads to building more robust and secure applications.
