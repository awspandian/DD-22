pipeline {
    agent any
 parameters {
	choice(name: 'BRANCH_TO_BUILD', choices: ['war', 'MASTER', 'k8s'], description: '') 
	string(name: 'MAVEN_GOAL', defaultValue: 'mvn install', description: 'maven build')
}
triggers { pollSCM('* * * * *') }

    stages {
        stage('SCM') {
            steps {
                git branch: "${params.BRANCH_TO_BUILD}", url: 'https://github.com/awspandian/DD-22.git'
            }
        }
 	stage('BUILD') {
            steps {
                bat "${params.MAVEN_GOAL}"
		
            }
        }
	stage('Deploy') {
            steps {
              deploy adapters: [tomcat9(credentialsId: 'web2', path: '', url: 'http://localhost:9090/')], contextPath: 'demo', war: '**/*.war'
		
            }
        }
    }
}
