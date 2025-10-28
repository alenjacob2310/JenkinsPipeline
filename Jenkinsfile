
pipeline {
  agent any

  tools {
    maven 'Maven3'     // Define this tool in Manage Jenkins → Tools (or remove tools{} and rely on mvn on PATH)
    jdk 'JDK17'        // Define JDK installation label similarly (optional if system Java is fine)
  }

  environment {
    MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"
  }

  stages {
    stage('Checkout') {
      steps {
        // For local path labs, Jenkins job SCM usually performs checkout.
        // This stage is a no-op placeholder when using multibranch/SCM here.
        echo "SCM checkout handled by job configuration."
      }
    }

    stage('Build') {
      steps {
        sh 'mvn -version'
        sh 'mvn -B -ntp clean compile'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn -B -ntp test'
      }
      post {
        always {
          junit '**/target/surefire-reports/*.xml'
        }
      }
    }

    stage('Package') {
      steps {
        sh 'mvn -B -ntp package'
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'target/*.war', fingerprint: true
      }
    }
  }

  post {
    success {
      echo 'BUILD SUCCESS: WAR is archived.'
    }
    failure {
      echo 'BUILD FAILED: check console logs.'
    }
  }
}
