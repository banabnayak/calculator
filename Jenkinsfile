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
        sh "docker build -t banabnayak/calculator:abc ."
      }
    }

    stage("Docker push") {
      steps {
	   sh "docker login -u banadipu -p bishnu12"

        sh "docker push banabnayak/calculator:abc"
      }
    }

    stage("Deploy to staging") {
      steps {
        sh "ansible-playbook playbook.yml -i inventory/staging"
        sleep 60
      }
    }

    stage("Acceptance test") {
      steps {
	sh "./acceptance_test.sh 18.224.67.101"
      }
    }
	  
    // Performance test stages

    stage("Release") {
      steps {
        sh "ansible-playbook playbook.yml -i inventory/production"
        sleep 60
      }
    }

    stage("Smoke test") {
      steps {
	sh "./smoke_test.sh 192.168.0.115"
      }
    }
  }
}
