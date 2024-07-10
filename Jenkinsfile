pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        UiPathPack (
          outputPath: "Output\\${env.BUILD_NUMBER}",
          projectJsonPath: "UiDemo_TestAutomation\\project.json",
          version: [$class: 'ManualVersionEntry', version: "1.0.${env.BUILD_NUMBER}"],
          useOrchestrator: true,
          orchestratorAddress: "https://cloud.uipath.com/galaxysoftwareservice/UATTenant/orchestrator_/",
          orchestratorTenant: "your-tenant-name",
          credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "2639310"]
        )
      }
    }
    stage('Deploy') {
      steps {
        UiPathDeploy (
          packagePath: "Output\\${env.BUILD_NUMBER}\\YourPackage.nupkg",
          orchestratorAddress: "https://cloud.uipath.com/galaxysoftwareservice/UATTenant/orchestrator_/",
          orchestratorTenant: "UATTenant",
          folderName: "UiDemo_TestAutomation",  // 這裡填入你的現代文件夾名稱
          environments: "Testing",  // 根據你的需要設置環境名稱
          credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "2639310"],
          traceLoggingLevel: 'None'
        )
      }
    }
    stage('Run Tests') {
      steps {
        UiPathRunTests (
          target: "Execute test set",
          testSet: "YourTestSet",
          orchestratorAddress: "https://cloud.uipath.com/galaxysoftwareservice/UATTenant/orchestrator_/",
          orchestratorTenant: "UATTenant",
          folderName: "UiDemo_TestAutomation",  // 這裡填入你的現代文件夾名稱
          credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "2639310"]
        )
      }
    }
  }
}


