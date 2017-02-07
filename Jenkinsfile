#!groovy

node {

	currentBuild.result = "SUCCESS"
	try{
	   def mvnHome
	   def java
	   
	   stage('Preparation') { // for display purposes
		  // Get some code from a GitHub repository
		  git 'https://github.com/muralibala/time-tracker.git'
		  // Get the Maven tool.
		  // ** NOTE: This 'M3' Maven tool must be configured
		  // ** in the global configuration.           
		  maven = tool 'M3'
		  java = tool 'jdk'
		  echo "git B: ${env.GIT_BRANCH}"
		  sh "env"
		  echo "Branch Name"
		  echo env.BRANCH_NAME
		  echo "Git Branch Name"
		  echo env.GIT_BRANCH 
		  echo "Building on branch: ${env.BRANCH_NAME}"
		  if (!isOnMaster()) {
			echo "Not master"
		  }
		  
	   }
	   
	   stage('Build') {
			withEnv (["JAVA_HOME=$java","PATH+MAVEN=$maven/bin:${env.JAVA_HOME}/bin"]) {
						  sh "mvn clean package"
					  } 
	   }
	   
	   stage('SonarQube analysis') {
			// requires SonarQube Scanner 2.8+
			def scannerHome = tool 'Sonar Scanner';
			withSonarQubeEnv('SonarQube') {
			  sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=TT -Dsonar.sources=."
			}
	   }
	   
	   stage ('Cleanup') {

            echo 'prune and cleanup'
            mail body: "project build successful - ${env.BUILD_URL}",
            from: 'i.am.muralibala@gmail.com',
            replyTo: 'i.am.muralibala@gmail.com',
            subject: "SUCCESS ${BUILD_NUMBER}",
            to: 'i.am.muralibala@gmail.com'
		}				
	}
	catch (err) {

        currentBuild.result = "FAILURE"

            mail body: "project build error is here: ${env.BUILD_URL}" ,
            from: 'i.am.muralibala@gmail.com',
            replyTo: 'i.am.muralibala@gmail.com',
            subject: "project build failed ${BUILD_NUMBER}",
            to: 'i.am.muralibala@gmail.com'

        throw err
    }
}

private boolean isOnMaster() {
    return !env.BRANCH_NAME || env.BRANCH_NAME == 'master';
}