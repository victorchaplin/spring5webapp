#!/usr/bin/groovy

node('BUILD') {

  def dockerArgs = "--privileged -u 0 \n\
    -v /home/svc_jenkins/workspace/settings.xml:/root/.m2/settings.xml \n\
    -v /var/run/docker.sock:/var/run/docker.sock \n\
    -v /home/svc_jenkins/.docker:/root/.docker";
    
  stage('Checkout Source') {
    checkout(scm)
  }

  withDockerContainer(args: dockerArgs, image: mavenImage) {
    stage('Adjust Version') {
      sh "mvn versions:set -DnewVersion=2.2.1-SNAPSHOT"
    }

    stage('Build/Test') {
      sh "mvn clean package -q"
    }
    
    if (env.BRANCH_NAME == mainBranch) {
      stage('Publish Artifact') {
        sh 'mvn deploy -DskipTests'
      }
    }

  }
  
}
