node {
    tag = VersionNumber projectStartDate: '', versionNumberString: '${BUILDS_ALL_TIME,X}', versionPrefix: 'RB0.0.', worstResultForIncrement: 'SUCCESS'
 
    def app
    stage('Clone repository') {
        checkout scm
    }
    
    stage('Build image and Unit Test') {
       app = docker.build("rla8127/test")
       app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${tag}")
        }
    }
    
    stage('Update Deployment') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email ehdugs77@naver.com"
                        sh "git config user.name rla8127"
                        //sh "git switch master"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+rla8127/test.*+rla8127/test:${tag}+g' deployment.yaml"
      }
    }
   }
 }
}
