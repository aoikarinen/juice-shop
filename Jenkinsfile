node {
    stage('Clone Repository') {
        checkout scm
    }    
   
  
   stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'sonarqubeScanner';
    withSonarQubeEnv('sonarqubeServer') {
      sh "${scannerHome}/bin/sonar-scanner  -Dsonar.sources=./routes"
    }
  }

    
    stage('Dependency Check') {
      environment {
          analyzer.bundle.audit.enabled=false
      }
      sh 'echo "Trying to run depedency check"'
      sh 'echo $analyzer.bundle.audit.enabled'
      dependencyCheckAnalyzer datadir: 'dependency-check-data', isFailOnErrorDisabled: true, hintsFile: '', includeCsvReports: false, includeHtmlReports: true, includeJsonReports: false, isAutoupdateDisabled: false, outdir: '', scanpath: 'package.json', skipOnScmChange: false, skipOnUpstreamChange: false, suppressionFile: '', zipExtensions: ''
      dependencyCheckPublisher canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
      archiveArtifacts allowEmptyArchive: true, artifacts: '**/dependency-check-report.*', onlyIfSuccessful: true
      step([$class: 'DependencyCheckPublisher', unstableTotalAll: '0'])
}

}
