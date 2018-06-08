node {
    stage('Clone Repository') {
        checkout scm
    }    
   
  
   stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'sonarqubeScanner';
    withSonarQubeEnv('sonarqubeServer') {
      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=annenpipeline -Dsonar.sources=./routes -Dsonar.host.url=http://10.48.253.181:9000 -Dsonar.login=50cf36311e42aec7843558c7ffddd4eac81fb97a"
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
