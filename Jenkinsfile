pipeline {
    agent none 

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        // work man
        maven "mymaven"
        jdk "myjava"
    }

    stages {
        stage('compile') {
            agent any
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/edureka-git/DevOpsClassCodes.git'

                // Run Maven on a Unix agent.
                sh "mvn compile"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }    
        stage('CodeReview') {
            agent any
            steps {
                // Run Maven on a Unix agent.
                sh "mvn pmd:pmd"
            }
        }
        
        stage('UnitTest') {
            agent {label 'slave-2'}
            steps {
                // Run Maven on a Unix agent.
                git 'https://github.com/edureka-git/DevOpsClassCodes.git'
                sh "mvn test"
            }
            post{
               always{
                   junit 'target/surefire-reports/*.xml'
               }
            }
        }
         stage('MetricCheck') {
            agent any
            steps {
                // Run Maven on a Unix agent.
                sh "mvn cobertura:cobertura -Dcobertura.report.format=xml"
            }
            post{
                always{
                    cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml'
                }
            }
        }
        
        stage('Packaging') {
            agent {label 'linux_slave'}
            steps {
                // Run Maven on a Unix agent.
                git 'https://github.com/edureka-git/DevOpsClassCodes.git'
                sh "mvn package"
            }
        }
    }
}
