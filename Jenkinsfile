def BRANCH = 'main'

pipeline {
    agent { label 'local' }
    tools {
        maven 'MVN-3-LOCAL'
        jdk 'JDK-21'
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
                echo bat(returnStdout: true, script: 'set')
                synopsys_scan product: 'polaris',
                      polaris_application_name: 'MKM-Demo', 
                      polaris_assessment_types: 'SAST', 
                      polaris_reports_sarif_create: true, 
                      polaris_reports_sarif_file_path: 'polaris.results.json', 
                      polaris_reports_sarif_groupSCAIssues: false, 
                      include_diagnostics: false
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'polaris.results.json, ".bridge/Polaris Coverity Capture/idir/build-log.txt", ".bridge/Polaris Coverity Capture/idir/capture-files-log.txt"'
            //cleanWs()
        }
    }
}
