pipeline {
    agent { 
        docker { image 'node:22.14.0-alpine3.21' } 
    }
    stages {
        stage('Setup') {
            steps {
                // Install Mocha and JUnit reporter
                sh 'npm install -g mocha mocha-junit-reporter'
                // Create a simple test file
                sh '''
                    mkdir -p test
                    echo "const assert = require('assert'); describe('Test', () => { it('should pass', () => { assert.equal(1, 1); }); });" > test/sample.test.js
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'node --version'
                sh 'echo "Building with Node.js..."'
            }
        }
        stage('Test') {
            steps {
                // Run Mocha with JUnit reporter, output XML to build/reports
                sh 'mocha test/sample.test.js --reporter mocha-junit-reporter --reporter-options mochaFile=build/reports/test-results.xml'
            }
        }
    }
    post {
        always {
            // Archive the XML reports
            archiveArtifacts artifacts: 'build/reports/**/*.xml', allowEmptyArchive: true
            // Process JUnit reports
            junit 'build/reports/**/*.xml'
        }
    }
}
