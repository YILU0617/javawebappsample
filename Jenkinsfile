import groovy.json.JsonSlurper

def getFtpPublishProfile(def publishProfilesJson) {
  def pubProfiles = new JsonSlurper().parseText(publishProfilesJson)
  for (p in pubProfiles)
    if (p['publishMethod'] == 'FTP')
      return [url: p.publishUrl, username: p.userName, password: p.userPWD]
}

node {
  withEnv([
  'AZURE_SUBSCRIPTION_ID=23941257-356f-4e1c-b3b7-c72126229939',
  'AZURE_TENANT_ID=a6f3caf2-09e9-4a92-bfed-17351d8fac9a'
]) {
  def resourceGroup = 'jenkins-get-started-rg'
  def webAppName   = 'YiLu-0617'

  withCredentials([usernamePassword(
    credentialsId: 'AzureServicePrincipal',
    usernameVariable: 'AZURE_CLIENT_ID',
    passwordVariable: 'AZURE_CLIENT_SECRET')]) {

    // 1) 先构建 WAR 包
    sh 'mvn -B -DskipTests clean package'

    // 2) 用服务主体登录并指定订阅
    sh '''
      az login --service-principal -u "$AZURE_CLIENT_ID" -p "$AZURE_CLIENT_SECRET" --tenant "$AZURE_TENANT_ID"
      az account set --subscription "$AZURE_SUBSCRIPTION_ID"
    '''

    // 3) 部署 WAR 到你的 Linux App Service
    sh """
      az webapp deploy --resource-group ${resourceGroup} \
        --name ${webAppName} --src-path target/calculator-1.0.war --type war
    """
  }
}
}
