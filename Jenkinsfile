pipeline {
    agent { 
        docker { image 'node:22.14.0-alpine3.21' } 
    }
    stages {
        stage('Setup') {
            steps {
                // Install locally instead of globally
                sh 'npm install mocha mocha-junit-reporter'
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
                // Run Mocha from local node_modules
                sh './node_modules/.bin/mocha test/sample.test.js --reporter mocha-junit-reporter --reporter-options mochaFile=build/reports/test-results.xml'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'build/reports/**/*.xml', allowEmptyArchive: true
            junit 'build/reports/**/*.xml'
        }
    }
}
