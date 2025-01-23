#!/usr/bin/env groovy

tools.createTrigger('abc123', 'github.com/ceros/jenkins-test-dummy', ['release/*'])
tools.initPipeline()

pipeline {
  agent
  {
    label 'jenkins-ecs-slave'
  }

  stages {
    stage ('Merge release into develop') {
      steps {
        script {
        
          tools.scmMerge("https://github.com/${repository}", ${branch}, 'feature/cer-1065')

        }
      }
    }
    
    stage('Push to develop') {
      // This is currently the best way to push a branch from a Pipeline 
      // job. https://issues.jenkins-ci.org/browse/JENKINS-28335 is an
      // open JIRA for getting the GitPublisher Jenkins functionality
      // working with Pipeline     
      steps {
        script {
          def creds = tools.getCredentials(CerosVars.GITHUB_CREDENTIALS_ID);
          sh "git push https://${creds}@github.com/${repository} HEAD:feature/cer-1065"    
            
        }
      }
    }
  }
}
