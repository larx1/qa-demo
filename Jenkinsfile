pipeline {
    agent any
    stages {
        stage('GIT Checkout') {
            steps {
                cleanWs()
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '2ba93103-b0b7-4f87-8fe3-672661870b90', url: 'git@github.com:larx1/qa-demo.git']]])
            }
        }
        
        stage('Performance tests') {
            steps {
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
