node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner for MSBuild'
    withSonarQubeEnv() {
      bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll begin /k:\"jwebapps\" /d:sonar.login="sqa_1526957e2e7fcd6400ece682360aa8213f1760da" " 
      bat "dotnet build"
      bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll end /d:sonar.login="sqa_1526957e2e7fcd6400ece682360aa8213f1760da" "
    }
  }
}
