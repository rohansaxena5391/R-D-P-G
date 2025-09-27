pipeline {
  agent any
  options {
        timeout(time: 4, unit: 'HOURS') 
    }   
  environment {
    APPSYSID = '1a162be49758b2941ea4f1e0f053afd6' 
    APPScope = '1280bf2c971cb2941ea4f1e0f053aff1'
    CREDENTIALS = 'servicenow'
    DEVENV = 'https://hclnowintelligence.service-now.com'
    PRODENV = 'https://ven03869.service-now.com'
  }
    

stages {
    stage('Build') {
      steps {
        script {
echo "Applying changes in DEV..."
        snApplyChanges(appSysId: "${APPSYSID}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
          echo "Publishing app from DEV..."
        def publishResult =snPublishApp(credentialsId: "${CREDENTIALS}", url: "${DEVENV}", appScope: "${APPScope}", appSysId: "${APPSYSID}",
          isAppCustomization: true, obtainVersionAutomatically: true,)

             /// Save published version to environment
env.PUBLISHED_VERSION = publishResult.appVersion
echo "Published version: ${env.PUBLISHED_VERSION}"
)     
        
      }
    }  
    
    stage('Install'){
           steps {
             script {
if (!env.PUBLISHED_VERSION?.trim()) {
error "No published version found. Did the Build stage succeed?"
}
echo "Installing app version ${env.PUBLISHED_VERSION} into PROD..."
snDevOpsChange(
credentialsId: "${CREDENTIALS}",
url: "${PRODEVN}"
)
           snDevOpsChange()
              snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}", publishedAppVersion: env.PUBLISHED_VERSION, baseAppAutoUpgrade: false)        
        
      }
    }
}

}
}

  post {
always {
echo "Pipeline finished. Version deployed: ${env.PUBLISHED_VERSION ?: 'N/A'}"
}
}
}
