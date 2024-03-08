node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner for MSBuild'
    withSonarQubeEnv() {
      bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll begin /k:\"jwebapps\" /d:sonar.login="<token>"" 
      bat "dotnet build"
      bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll end /d:sonar.login="<token>""
    }
  }
}
