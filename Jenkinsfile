node {
    //First step is to checkout (clone) the repository to make sure we are working with the latest
    stage('Clone Repository') {
      checkout scm
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
//    stage('SCM') {
//    git 'https://github.com/edubarrachina/juice-shop/.git'
//    }
    stage('SonarQube analysis') {
        // requires SonarQube Scanner 2.8+
        def scannerHome = tool 'sonarqubeScanner';
        withSonarQubeEnv('sonarqubeServer') {
          sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=EduDISEC -Dsonar.sources=./app -Dsonar.host.url=http://10.48.253.181:9000 -Dsonar.login=aaf0263cb71d6255e248d3e42e382bc42da6c8a5"
        }
      }
}
