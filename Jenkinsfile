def branch

def getDeploymentEnvironment() {
    if (env.BRANCH_NAME.startsWith('PR-')) {
        return 'pr'
    } else {
	    return env.BRANCH_NAME
    }   
}

pipeline{
	agent any

	stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
                echo 'Cleeanup done'
            }
        }  

        stage('Checkout'){
            steps {
        	checkout scm
			    script {
				    branch = "${getDeploymentEnvironment()}"
				    echo "the present branch is: ${branch}"
			    	}
        	}
         }
	
		stage('Compile') {
            steps {
            	script {
            			sh 'mvn --version'
            			sh 'mvn clean compile'
            	}
                
            }
        }  

        stage('Test') {
            steps {
                script {
                        sh 'mvn --version'
                        sh 'mvn test'
                }
                
            }
        }  

        stage('Deployment') {
            steps {
                script {
                        sh 'sudo sh /root/deploy.sh'
                }
                
            }
        }  

		
    }
}
