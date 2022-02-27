pipeline{
  agent any
  tools{ maven 'maven'}
  stages{
    stage('git clone'){
      steps{
        git branch: 'main', credentialsId: 'Jenkins-GITHUB-CREDENTIALS', url: 'https://github.com/honey336/module2_ci_Elv'
      }
    }
    stage('Build Artifact-Maven'){
      steps{
      sh "mvn clean package -DskipTests=true"
      archive 'target/*.jar'
      }
    }
    stage('Unit Tests - JUnit and JaCoCo') {
      steps {
        sh "mvn test"
        sh "mvn -v"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
    stage('Mutation Tests - PIT') {
      steps {
        sh "mvn org.pitest:pitest-maven:mutationCoverage"
      }
      post {
        always {
          pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
        }
      }
    }
    stage('CodeQuality-SAST'){
      steps {
        sh 'mvn clean verify sonar:sonar \
  -Dsonar.projectKey=devsecops-springapp \
  -Dsonar.host.url=http://honey-devops.eastus.cloudapp.azure.com:9000 \
  -Dsonar.login=cf7ee7a97d610405d9b9afc91177c688ee8946c0'
      }
    } 
  }
}
