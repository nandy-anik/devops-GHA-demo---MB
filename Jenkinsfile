#!/usr/bin/env groovy

import java.time.LocalDateTime
import java.time.format.DateTimeFormatter
import java.time.Duration
import java.time.ZoneId
import java.time.temporal.ChronoUnit

node {
    stage('checkout') {
        final scmVars = checkout scm
    }

    stage('check java') {
        sh "java -version"
    }

    stage('clean') {
        sh "chmod +x mvnw"
        sh "./mvnw -ntp clean -P-webapp"
    }
    stage('nohttp') {
        sh "./mvnw -ntp checkstyle:check"
    }

    stage('install tools') {
        sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm@install-node-and-npm"
    }

    stage('npm install') {
        nodejs(nodeJSInstallationName: 'Node 16', configId: 'a6d9fd61-b44f-4203-879e-11379401e2d3') {
            sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm"
        }
        
    }
    stage('backend tests') {
        try {
            sh "./mvnw -ntp verify -P-webapp"
        } catch(err) {
            throw err
        } finally {
            junit '**/target/surefire-reports/TEST-*.xml,**/target/failsafe-reports/TEST-*.xml'
        }
    }

    stage('frontend tests') {
        try {
            sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm -Dfrontend.npm.arguments='run test-ci'"
        } catch(err) {
            throw err
        } finally {
            junit '**/target/test-results/TESTS-results-jest.xml'
        }
    }

    stage('quality analysis') {
        withSonarQubeEnv('now-sonar') {
            sh "./mvnw -ntp initialize sonar:sonar"
        }
    }

    stage('packaging') {
        //sh "./mvnw -ntp verify -P-webapp deploy -Pprod -DskipTests"
        //archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        echo "Register artifact in ServiceNow"
        
        def branchName = env.BRANCH_NAME
        
//        snDevOpsArtifact artifactsPayload: """
//        {
//            "artifacts": [{
//                "name": "devops-java-demo.jar",
//                "version": "0.0.2-SNAPSHOT+${currentBuild.number}",
//                "semanticVersion": "0.0.2-SNAPSHOT+${currentBuild.number}",
//                "repositoryName": "devops-java-demo"
//            }],
//            "branchName": "${branchName}"
//        }
//        """
        
        //if (env.BRANCH_NAME == 'main') {
            echo "Create package in ServiceNow"
//            snDevOpsPackage name: "devops-java-demo", artifactsPayload: """
//            {
//                "artifacts": [{
//                    "name": "devops-java-demo.jar",
//                    "version": "0.0.2-SNAPSHOT+${currentBuild.number}",
//                    "repositoryName": "devops-java-demo"
//                }],
//                "branchName": "${branchName}"
//            }
//            """
        //}
    }

    if (env.BRANCH_NAME != 'main') {
        stage('deploy') {

            echo "Create change request in ServiceNow..."

            def plannedStartStr = getFormattedDate(2)
            def plannedEndStr = getFormattedDate(8)

            echo "Change request scheduled between ${plannedStartStr} UTC and ${plannedEndStr} UTC"
            
//            snDevOpsChange changeRequestDetails: """
//            {
//                "setCloseCode": true,
//                "attributes": {
//                    "start_date": "${plannedStartStr}",
//                    "end_date": "${plannedEndStr}",
//                    "u_service_instance": "d7d4fcc51bd608d09a49657f7b4bcb30",
//                    "cmdb_ci": "d7d4fcc51bd608d09a49657f7b4bcb30",
//                    "short_description": "Deploy build #${currentBuild.number} to non-production",
//                    "description": "More detailed description for build #${currentBuild.number}",
//                    "requested_by": "c2e855311b1a0550a7bf7553dd4bcb25"
//                }
//            }
//            """
            echo "...simulate non-production deployment"
        }
    }
    
    if (env.BRANCH_NAME == 'main') {
        stage('prod deploy') {

            echo "Create change request in ServiceNow..."

            def plannedStartStr = getFormattedDate(2)
            def plannedEndStr = getFormattedDate(8)

            echo "Change request scheduled between ${plannedStartStr} UTC and ${plannedEndStr} UTC"
            
//            snDevOpsChange changeRequestDetails: """
//            {
//                "setCloseCode": true,
//                "attributes": {
//                    "start_date": "${plannedStartStr}",
//                    "end_date": "${plannedEndStr}",
//                    "u_service_instance": "d7d4fcc51bd608d09a49657f7b4bcb30",
//                    "cmdb_ci": "d7d4fcc51bd608d09a49657f7b4bcb30",
//                    "short_description": "Deploy build #${currentBuild.number} to production",
//                    "description": "More detailed description for build #${currentBuild.number}",
//                    "requested_by": "c2e855311b1a0550a7bf7553dd4bcb25"
//                }
//            }
//            """
            echo "...simulate production deployment"
        }
    }
}

@NonCPS
String getFormattedDate(int offset) {
    def time = LocalDateTime.now(ZoneId.of("UTC")).plus(Duration.of(offset, ChronoUnit.MINUTES));
    def formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
    return time.format(formatter)
}
