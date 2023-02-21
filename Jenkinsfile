// Run the job every 5 minutes 
CRON_SETTINGS = '''H/30 * * * *'''
pipeline {
    agent any
    triggers {
        cron(CRON_SETTINGS)
    }
    environment {
        // [Mandatory]
        // JFrog platform URL (This functionality requires version 3.29.0 or above of Xray)
        JF_URL = credentials("JF_URL")

        // [Mandatory if JF_USER and JF_PASSWORD are not provided]
        // JFrog access token with 'read' permissions for Xray
        JF_ACCESS_TOKEN= credentials("JF_ACCESS_TOKEN")

    
        // [Mandatory]
        // GitHub enterprise server access token with the following permissions:
        // Read and Write access to code, pull requests, security events, and workflows
        JF_GIT_TOKEN = credentials("FROGBOT_GIT_TOKEN")
        JF_GIT_PROVIDER = "github"

        // [Mandatory]
        // GitHub enterprise server organization namespace
        JF_GIT_OWNER = "nagag08"
        JF_GIT_REPO = "frogbot"

        // [Mandatory]
        // API endpoint to GitHub enterprise server
        JF_GIT_API_ENDPOINT = ""

        // [Mandatory if JF_ACCESS_TOKEN is not provided]
        // JFrog user and password with 'read' permissions for Xray
        // JF_USER = credentials("JF_USER")
        // JF_PASSWORD = credentials("JF_PASSWORD")
        }
        tools {
           maven 'MAVEN_TOOL'
       }
        stages {
            stage('Download Frogbot') {
                steps {
                    // For Linux / MacOS runner:
                    sh """ curl -fLg "https://releases.jfrog.io/artifactory/frogbot/v2/[RELEASE]/getFrogbot.sh" | sh"""
                    // For Windows runner:
                    // powershell """iwr https://releases.jfrog.io/artifactory/frogbot/v2/[RELEASE]/frogbot-windows-amd64/frogbot.exe -OutFile .\frogbot.exe"""
                }
            }
            stage('Scan Pull Requests') {
                steps {
                    sh "./frogbot scan-pull-requests"
                    // For Windows runner:
                    // powershell """.\frogbot.exe scan-pull-requests"""
                }
            }
            stage('Scan and Fix Repos') {
                 steps {
                     sh "./frogbot scan-and-fix-repos"
                     // For Windows runner:
                     // powershell """.\frogbot.exe scan-and-fix-repos"""
                 }    
            }    
        }
    }
