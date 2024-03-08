node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner for MSBuild'
    withSonarQubeEnv() {
      bat "dotnet C:\\ProgramData\\chocolatey\\lib\\sonarscanner-net\\tools\\SonarScanner.MSBuild.dll begin /k:\"jwebapps\""
      bat "dotnet build C:\\Users\\admin\\source\\repos\\Jwebapps\\Jwebapps.csproj"
      bat "dotnet C:\\ProgramData\\chocolatey\\lib\\sonarscanner-net\\tools\\SonarScanner.MSBuild.dll end"
    }
  }
}
