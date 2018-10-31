pipeline {
  agent any
	
  triggers {
    pollSCM('* * * * *')
  }
	
  stages {
    stage("Compile") {
      steps {
        sh "./gradlew compileJava"
      }
    }
	  
    stage("Unit test") {
      steps {
        sh "./gradlew test"
      }
    }
	
    stage("Build") {
      steps {
        sh "./gradlew build"
      }
    }

    stage("Docker build") {
      steps {
        sh "docker build -t banadipu/calculator_1 ."
      }
    }

    stage("Docker push") {
      steps {
	   sh "docker login -u banadipu -p bishnu12"

        sh "docker push banadipu/calculator_1"
      }
    }

    stage("Deploy to staging") {
      steps {
       sh "docker run -d --rm -p 8765:8080 --name calculator banadipu/calculator_1"
      }
    }
	  
    
  }
}
