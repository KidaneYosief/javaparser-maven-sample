#!/usr/bin/env groovy
def BranchList = ["master", "dev", "stage"]
def branch = "${env.BRANCH_NAME}"
// def PRO_VERSION = "1.0"
pipeline {
	agent any
	options {
        ansiColor('xterm')
    }
	environment { 
		LANG="en_US.UTF-8"
    }
    stages {
		stage ('Update POM') {
			steps {
				script { 
					if (branch == 'master'){
						sh '''
							export PRO_VERSION1=`mvn org.apache.maven.plugins:maven-help-plugin:3.2.0:evaluate -Dexpression=project.version -q -DforceStdout`
							echo ${PRO_VERSION1}
							mvn versions:set -DnewVersion="${PRO_VERSION1}"-${BUILD_ID} -s settings.xml
						'''
					} else {
						sh '''
							export PRO_VERSION1=`mvn org.apache.maven.plugins:maven-help-plugin:3.2.0:evaluate -Dexpression=project.version -q -DforceStdout`
							mvn versions:set -DnewVersion=${PRO_VERSION1}-SNAPSHOT -s settings.xml
							cat pom.xml
						'''
					}
				}
			}
		}
		stage ('Build project') {
			steps {
				script { 
					if (branch == 'master' || branch == 'dev'){
						sh '''
							mvn clean install
						'''
					} else {
						sh '''
							mvn clean install
						'''
					}
				}
			}
		}
	}
	post {
		always {
			cleanWs()
		}
	}
}
