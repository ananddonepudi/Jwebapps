pipeline {
    agent any
    environment {
        dotnet = 'C:\\Program Files\\dotnet\\dotnet.exe'
    }
    stages {
        stage('Checkout Stage') {
            steps {
                    git credentialsId: '3aff180a-c94e-44f1-b599-efa5b76c5351', url: 'https://github.com/ananddonepudi/Jwebapps.git'
				
				   }
                }
         stage('Clean Stage') {
            steps {
               dotnetClean project: 'C:\\Users\\admin\\source\\repos\\Jwebapps'
            }
        }
        stage('Build Stage') {
            steps {
                dotnetBuild project: 'C:\\Users\\admin\\source\\repos\\Jwebapps'
            }
          }
        stage('UnitTest Stage') {
            steps {
                dotnetTest project:'C:\\Users\\admin\\source\\repos\\ananddonepudi\\AspNetCoreTests\\AspNetCoreTests\\AspNetCoreTests.UnitTests'
            }
        }
        stage("Release Stage") {
            steps {
             s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 's3-ad-artifcatjdemo', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'ap-south-1', showDirectlyInBrowser: false, sourceFile: '**/*', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'Demoprofile', userMetadata: []
            }
        }
        stage('SonarQube Analysis') {
             steps {
                        bat "dotnet tool install --global dotnet-sonarscanner"
                        bat "dotnet C:\\Users\\admin\\Sonar-scanner\\SonarScanner.MSBuild.dll begin /k:\"sonarsc-jwebapps\" /d:sonar.login=\"sqa_5956be524ca099dd670f603ce057157cb32927f2\" "
                        bat "dotnet build"
                        bat "dotnet C:\\Users\\admin\\Sonar-scanner\\SonarScanner.MSBuild.dll end /d:sonar.login=\"sqa_5956be524ca099dd670f603ce057157cb32927f2\" "
                          
                    }
  }
              stage('Snyk Testing') {
      steps {
        echo 'Testing...'
        snykSecurity(
           snykInstallation: 'mysnyk',
           snykTokenId: 'SnykAPItoken'
        )
      }
    }
        stage('Deploy Stage') {
            steps {
             dotnetPublish project: 'C:\\Users\\admin\\source\\repos\\Jwebapps', selfContained: false
            }
        }
       
        stage('Integration Test Stage') {
            steps {
                dotnetTest project:'C:\\Users\\admin\\source\\repos\\ananddonepudi\\AspNetCoreTests\\AspNetCoreTests\\AspNetCoreTests.IntegrationTests'
            }
        }
          stage('Organise to  Staging'){  
              steps{
                  bat 'xcopy  /s /i  "C:\\Users\\admin\\source\\repos\\Jwebapps\\bin\\Release\\net6.0\\publish" "C:\\Anand\\staging" /Y'
                   }
               }
               stage('Integration Test') {
            steps {
                echo 'Integrating test'
            }
        } 
           stage('Organise Files Production'){  
              steps{
                  bat 'xcopy  /s /i  "C:\\Users\\admin\\source\\repos\\Jwebapps\\bin\\Release\\net6.0\\publish" "C:\\Anand\\production" /Y'
                   }
               }        
    }
}
