node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'GitHub', 
                                                 passwordVariable: 'GIT_PASSWORD', 
                                                 usernameVariable: 'GIT_USERNAME')]) {
                    // Configure Git identity
                    sh 'git config user.email "mothukurirk@gmail.com"'
                    sh 'git config user.name "Radhakrishna Mothukuri"'

                    // Confirm the YAML path & show before editing
                    sh 'cat kubernetesmanifest/deployment.yaml'

                    // Replace old image tag with the new one
                    sh '''
                        sed -i "s#mothukurirk/test:latest#mothukurirk/test:${DOCKERTAG}#g" kubernetesmanifest/deployment.yaml
                    '''

                    // Show the updated YAML
                    sh 'cat kubernetesmanifest/deployment.yaml'

                    // Commit and push changes
                    sh 'git add .'
                    sh 'git commit -m "Jenkins updated manifest with new image tag: ${env.BUILD_NUMBER}" || true'
                    sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/jenkinsfile-wordpress.git'
                }
            }
        }
    }
}

