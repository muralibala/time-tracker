node {

	currentBuild.result = "SUCCESS"
	try{
	   def mvnHome
	   def java
	   stage('Preparation') { // for display purposes
		  // Get some code from a GitHub repository
		  git 'https://github.com/srctips/time-tracker.git'
		  // Get the Maven tool.
		  // ** NOTE: This 'M3' Maven tool must be configured
		  // **       in the global configuration.           
		  maven = tool 'M3'
		  java = tool 'jdk'
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
            mail body: 'project build successful',
            from: 'i.am.muralibala@gmail.com',
            replyTo: 'i.am.muralibala@gmail.com',
            subject: 'project build failed',
            to: 'i.am.muralibala@gmail.com'
		}				
	}
	catch (err) {

        currentBuild.result = "FAILURE"

            mail body: "project build error is here: ${env.BUILD_URL}" ,
            from: 'i.am.muralibala@gmail.com',
            replyTo: 'i.am.muralibala@gmail.com',
            subject: 'project build failed',
            to: 'i.am.muralibala@gmail.com'

        throw err
    }
}