pipeline {
  agent any
  environment {
    ORCHESTRATOR_ADDRESS = "https://cloud.uipath.com/galaxysoftwareservice/UATTenant/orchestrator_/"
    ORCHESTRATOR_TENANT = "UATTenant"
    CREDENTIALS_ID = "UiPath_UAT_APIAccess"
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/SiaPun/UiDemo_TestAutomation'
      }
    }
    stage('Build') {
      steps {
        UiPathPack (
          outputPath: "Output\\${env.BUILD_NUMBER}",
          projectJsonPath: "UiDemo_TestAutomation\\project.json",
          version: [$class: 'ManualVersionEntry', version: "1.0.${env.BUILD_NUMBER}"],
          useOrchestrator: true,
          traceLevel: 'None',
          orchestratorAddress: "${env.ORCHESTRATOR_ADDRESS}",
          orchestratorTenant: "${env.ORCHESTRATOR_TENANT}",
          credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "${env.CREDENTIALS_ID}"]
        )
      }
    }
    stage('Deploy') {
      steps {
        UiPathDeploy (
          packagePath: "Output\\${env.BUILD_NUMBER}\\YourPackage.nupkg",
          orchestratorAddress: "${env.ORCHESTRATOR_ADDRESS}",
          orchestratorTenant: "${env.ORCHESTRATOR_TENANT}",
          folderName: "UiDemo_TestAutomation",
          environments: "Testing",
          credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "${env.CREDENTIALS_ID}"],
          traceLevel: 'None'
        )
      }
    }
    stage('Run Tests') {
      steps {
        UiPathRunTests (
          target: "Execute test set",
          testSet: "UIDEMO:3",
          orchestratorAddress: "${env.ORCHESTRATOR_ADDRESS}",
          orchestratorTenant: "${env.ORCHESTRATOR_TENANT}",
          folderName: "UiDemo_TestAutomation",
          credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "${env.CREDENTIALS_ID}"]
        )
      }
    }
  }
}
