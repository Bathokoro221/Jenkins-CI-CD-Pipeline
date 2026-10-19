// Jenkinsfile - Declarative Pipeline
// This file defines our entire CI process as code
// It must be in the ROOT of your GitHub repo

pipeline {
    // agent any means Jenkins can use any available worker to run this job
    agent any

    // tools - Jenkins will auto-install these tools (configure in Manage Jenkins > Tools)
    // tools {
    //     nodejs 'node18' // for Node.js projects
    //     jdk 'jdk17'     // for Java projects
    // }

    // environment variables - available in all stages
    environment {
        APP_NAME = 'my-app' // name of your application
        // NEVER put passwords here directly - use Jenkins Credentials
        // Example: GITHUB_TOKEN = credentials('github-pat')
    }

    // options for better pipeline behavior
    options {
        // Keep only last 10 builds to save disk space
        buildDiscarder(logRotator(numToKeepStr: '10'))
        // Show timestamp in console logs for debugging
        timestamps()
    }

    // stages - the main steps of CI
    stages {
        
        // STAGE 1: Checkout - Get latest code from GitHub
        stage('Checkout') {
            steps {
                echo 'Step 1: Pulling latest code from repository...'
                // checkout scm automatically checks out code from the repo configured in Jenkins job
                checkout scm
            }
        }
        
        // STAGE 2: Build - Install dependencies and compile app
        stage('Build') {
            steps {
                echo "Step 2: Building ${env.APP_NAME}..."
                // For Node.js: npm ci installs dependencies cleanly
                // sh 'npm ci' 
                // sh 'npm run build'
                
                // For Python:
                // sh 'pip install -r requirements.txt'
                
                // For Java Maven:
                // sh 'mvn clean package -DskipTests'

                // Dummy command for demo - REPLACE with real build command above
                sh 'echo "Build successful"'
            }
        }
        
        // STAGE 3: Automated Testing - Verify code stability
        stage('Test') {
            steps {
                echo 'Step 3# Jenkins CI/CD Pipeline Project

## Jenkins Setup
- Environment: Docker LTS running on localhost:8080
- Java Version: OpenJDK 17
- Plugins Used: Git, Pipeline, Blue Ocean, Credentials Binding, JUnit

## Pipeline Stages Explanation
1. **Checkout:** Uses `checkout scm` to pull latest commit from main branch
2. **Build:** Installs dependencies and compiles/package app
3. **Test:** Executes unit tests and publishes JUnit results
4. **Package:** Creates Docker image / archives JAR as artifact

## Trigger
Configured via GitHub Webhook on Push events. Fallback Poll SCM every 2 mins.

## Credentials Security
Used Jenkins Credentials store for 【entity-GitHub¦canonical_name=GitHub】 PAT, accessed via credentials() helper.

## Successful Build
See /screenshots folder