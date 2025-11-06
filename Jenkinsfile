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
    
    sh 'mvn -B -DskipTests clean package'


    sh '''
      az login --service-principal -u "$AZURE_CLIENT_ID" -p "$AZURE_CLIENT_SECRET" --tenant "$AZURE_TENANT_ID"
      az account set --subscription "$AZURE_SUBSCRIPTION_ID"
    '''

  
    sh """
      az webapp deploy --resource-group ${resourceGroup} \
        --name ${webAppName} --src-path target/calculator-1.0.war --type war
    """
  }
}
