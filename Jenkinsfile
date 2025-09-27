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
        snApplyChanges(appSysId: "${APPSYSID}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
        snPublishApp(credentialsId: "${CREDENTIALS}", url: "${DEVENV}", appScope: "${APPScope}", appSysId: "${APPSYSID}",
          isAppCustomization: true, obtainVersionAutomatically: false,) 
       
        
      }
    }  
    
    stage('Install'){
           steps {
              snDevOpsChange()
              snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}", baseAppAutoUpgrade: false)        
        
      }
    }
}

}
