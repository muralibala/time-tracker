node {
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
		withSonarQubeEnv('My SonarQube Server') {
		  sh "${scannerHome}/bin/sonar-scanner"
		}
   }
}