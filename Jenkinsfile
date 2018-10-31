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
        sh "docker build -t banadipu/calculator:abc ."
      }
    }

    stage("Docker push") {
      steps {
	   sh "docker login -u banadipu -p bishnu12"

        sh "docker push banadipu/calculator:abc"
      }
    }

    stage("Deploy to staging") {
      steps {
       sh "docker run -d --rm -p 8765:8080 --name calculator:abc banadipu/calculator:abc"
      }
    }

    stage("Acceptance test") {
      steps {
	 sleep 60
          sh "./acceptance_test.sh"
      }
    }
	  
    
  }
}
