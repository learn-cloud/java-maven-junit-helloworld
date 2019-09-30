def branch

def getDeploymentEnvironment() {
    if (env.BRANCH_NAME.startsWith('PR-')) {
        return 'PR'
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
				    sh 'touch version.txt'
				    env.WORKSPACE = pwd()
				    def fh2 = readFile "${env.WORKSPACE}/version.txt"
				    // File fh2 = new File "${env.WORKSPACE}/version.txt"
					def lines = fh2.readLines()
					for (line in lines) {
					    folder = line
					    println folder
					   }
				    
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

        stage('Dev Deployment') {

            when {
                expression { env.BRANCH_NAME == 'dev' }
            }

            steps {
                script {
                        sh 'sudo sh /root/deploy.sh'
                        echo "Deployment Done in Dev"
                }
                
            }
        }  

        stage('Master Deployment') {

            when {
                expression { env.BRANCH_NAME == 'master' }
            }

            steps {
                script {
                        sh 'sudo sh /root/deploy.sh'
                        echo "Deployment Done in Master"
                }
                
            }
        }  

        stage('PR Deployment') {

            when {
                expression { branch == 'PR' }
            }

            steps {
                script {

                        echo "Only Test Cases are runnig...Ignoring all the conditions"
                }
                
            }
        }  

		
    }
}
