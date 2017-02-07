#!groovy

node {

	currentBuild.result = "SUCCESS"
	try{
	   def mvnHome
	   def java
	   def branch = "${BRANCH_NAME}"
	   
	   stage('Preparation') { // for display purposes
		  // Get some code from a GitHub repository
		  git 'https://github.com/muralibala/time-tracker.git'
		  // Get the Maven tool.
		  // ** NOTE: This 'M3' Maven tool must be configured
		  // ** in the global configuration.           
		  maven = tool 'M3'
		  java = tool 'jdk'
		  echo "My branch is: ${env.BRANCH_NAME}"
		  echo "git B: ${env.GIT_BRANCH}"
		  echo branch
		  sh 'echo $BRANCH_NAME'
		  
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