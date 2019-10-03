def branch

def getDeploymentEnvironment() {
    if (env.BRANCH_NAME.startsWith('PR-')) {
        return 'PR'
    } else {
	    return env.BRANCH_NAME
    }   
}

def name = 'test'

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
				    sh 'echo "hello anand" > version.txt'
				    echo "------------------"
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
            			// sh 'mvn clean compile'
			        sh 'ls -ltr'
			        sh 'pwd'
			        sh 'cd test'
			        sh 'ls -ltr'
			        sh 'sudo sh test/hello.sh'
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

        stage('PRR Deployment') {

            when {
        		allOf{
                   		// expression { name == 'test'}
				expression { branch == 'PR'}
			}
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
