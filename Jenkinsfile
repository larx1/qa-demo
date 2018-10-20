pipeline {
    agent any
    stages {
        stage('Performance tests') {
            steps {
            cleanWs()     
	    withEnv(['JMETER_HOME=/home/roberto/apache-jmeter-4.0/bin']) {
                    sh '$JMETER_HOME/jmeter -n -t api_tests.jmx -l results.jtl'
                }
            }
        }
        stage('Generate dashboard') {
            steps {
                withEnv(['JMETER_HOME=/home/roberto/apache-jmeter-4.0/bin']) {
                    sh '$JMETER_HOME/jmeter -g results.jtl -o dashboard'
                }
            }
        }
    }
    post {
        always {
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'dashboard', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
            perfReport percentiles: '0,50,90,100', sourceDataFiles: 'results.jtl'
        }
    }
}
