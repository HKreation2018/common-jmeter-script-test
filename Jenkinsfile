pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS_USR = "hari-jmeter"
	DEPLOY_CREDS_PSW = "Indian@018"
    MULE_VERSION = '4.3.0'
    BG = "IFT"
    WORKER = "Micro"
  }
  stages {
    stage('Build') {
      steps {
            bat 'mvn -B -U -e -V clean -DskipTests package'
      }
    }

   /* stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'common-jmeter-script-test'
      }
      steps {
            bat 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS_USR%" -Danypoint.password="%DEPLOY_CREDS_PSW%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.worker="%WORKER%"'
      }
    } */
	
	stage('performance test') {       
      steps {
             bat 'mvn verify -DthreadCount=${THREADS} -DrampupTime=${RAMPUP} -DdurationSecond=${DURATION} -DloopCountNo=${LOOPCOUNT} -Dcsvfile=${CSVFILENAME}'
      }
	  
	  post {
        always {
            archiveArtifacts artifacts: 'target/jmeter/results/*.csv', caseSensitive: false, defaultExcludes: false, followSymlinks: false, onlyIfSuccessful: true
			perfReport 'target/jmeter/results/*.csv'
			publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'target/jmeter/reports/Test Plan', reportFiles: 'index.html', reportName: 'Performance Report', reportTitles: ''])
        }
	}
    }
  }
}
