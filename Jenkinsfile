pipeline {
agent any
options {
timeout(time: 4, unit: 'HOURS')
}

environment {
APPSYSID = '1a162be49758b2941ea4f1e0f053afd6'
APPSCOPE = 'x_hclte_r_d_p_g'
CREDENTIALS = 'servicenow'
DEVEN = 'https://hclnowintelligence.service-now.com'
PRODEVN = 'https://vena3869.service-now.com'
}

stages {
stage('Build') {
steps {
script {
echo "Applying changes in DEV..."
snApplyChanges(
appSysId: "${APPSYSID}",
url: "${DEVEN}",
credentialsId: "${CREDENTIALS}"
)

echo "Publishing app from DEV..."
def publishResult = snPublishApp(
credentialsId: "${CREDENTIALS}",
url: "${DEVEN}",
appScope: "${APPSCOPE}",
appSysId: "${APPSYSID}",
isAppCustomization: true,
obtainVersionAutomatically: true
)

// Save published version to environment
env.PUBLISHED_VERSION = publishResult.appVersion
echo "Published version: ${env.PUBLISHED_VERSION}"
}
}
}

stage('Install') {
steps {
snDevOpsChange()
snInstallApp(
credentialsId: "${CREDENTIALS}",
url: "${PRODEVN}",
appSysId: "${APPSYSID}",
publishedAppVersion: "${env.PUBLISHED_VERSION}",
baseAppAutoUpgrade: false
)
}
}
}
}
