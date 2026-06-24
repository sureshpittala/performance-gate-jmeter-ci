pipleine {
  agent any
//checkout stage
  stages ('Checkout') {
    steps {checkout scm}
  }
  //JMeter execution stage
  stage('JMeter') {
    steps {
      bat ''' // to execute multiple commands
      if exists logs rmdir /s /q logs
      if exists html rmdir /s /q html
      mkdir logs
      mkdir html
      mkdir html\\report
      "C:\\jmeter\\apache-jmeter-5.6.3\\bin" -n -t API.jmx -l logs/results.jtl -e -o html/report
      ''' // to complete bat block
    } // to complete the steps block
  } // to complete the stage block
  
  // configure the performance gate
  stage('Performance Gates') {
    steps { // configure the performance reports plugin
      perfReport {
        // define the source data file
        sourceDataFile: 'logs/results.jtl',
        // define the absolute error threshold
         errorFailedThreshold: 1.0, //build fails if error rate exceeds 1%
          errorUnstableThreshold: 0.0, //marks unstable if any error occurs
          //Double number (no quotes!) this indicates threshold values must be double numbers without quotes
          relativeFailedThresholdPositive: 1.3, // this failes build if response times degrade by 30%
          relativeUnstableThresholdPostive: 1.2, // this marks the build unstable if response times degrade by 20%
          modeOfthreshold: true,
          configType: 'ART',
          }
    }
  }
  stage('Reports') {
    steps { // configure the Performance Reports
      perfReport sourceDataFiles: 'logs/results.jtl'
      publishHTML(
        target: [
          reportDir: 'html/report',
          reportFiles: 'index.html',
          reportName: 'JMeter HTML Report',
          keepAll: true,
          alwaysLinkToLastBuild: true,
          allowMissing: false
          ]
        )
      }
    }
  }
  // Configure post-build actions
 post {
   always {
     archiveArtifacts 'logs/results.jtl, html/report/**'
   }
 }
}
  
      
      
          
          
  
