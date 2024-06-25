def BRANCH = 'main'

pipeline {
    agent { label 'linux64' }
    tools {
        maven 'MVN-3-LOCAL'
        jdk 'JDK-17'
    }
    stages {
        stage('checkout') {
            steps {
                cleanWs()
                checkout scm
            }
        }
        stage('polaris') {
            steps {
                synopsys_scan product: 'polaris',
                      polaris_application_name: 'MKM-Demo', 
                      polaris_assessment_types: 'SAST', 
                      polaris_reports_sarif_create: true, 
                      polaris_reports_sarif_file_path: 'polaris.results.json', 
                      polaris_reports_sarif_groupSCAIssues: true, 
                      include_diagnostics: true
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '.synopsys/polaris/configuration/synopsys.yml, .synopsys/polaris/data/coverity/*/idir/build-log.txt'
            cleanWs()
        }
    }
}
