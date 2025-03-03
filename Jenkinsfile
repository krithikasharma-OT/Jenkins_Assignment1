pipeline{ 
    agent any 
    environment{ 
     GIT_CREDENTIALS = credentials('Github_Username')
     SLACK_TOKEN=credentials('SlackCred')
    } 
    stages{ 
        stage('Clone Repo and cd and init'){ 
            steps{ 
                sh ''' 
                if [ -d "Jenkins_Assignment1" ]; then 
                    rm -rf Jenkins_Assignment1 
                fi 
                git clone  https://${GIT_CREDENTIALS}@github.com/krithikasharma-OT/Jenkins_Assignment1.git 
                ''' 
            } 
        } 
        stage('create and push branch'){ 
            steps{ 
                dir('Jenkins_Assignment1'){ 
                    sh ''' 
                    git checkout -b new-feature-branch 
                    git push origin new-feature-branch 
                ''' 
                } 
            } 
        } 
        stage('List Branch'){ 
            steps{ 
                dir('Jenkins_Assignment1'){ 
                    sh ''' 
                    git branch -r 
                    ''' 
                } 
            } 
        } 
        stage('Merge'){ 
            steps{ 
                dir('Jenkins_Assignment1'){ 
                    sh ''' 
                    git checkout main 
                    git pull origin main 
                    git merge new-feature-branch 
                    git push origin main 
                    ''' 
                } 
            } 
        } 
        stage('Rebase'){ 
            steps{ 
                dir('Jenkins_Assignment1'){ 
                    sh ''' 
                    git checkout main 
                    git rebase new-feature-branch 
                    git push --force origin main 
                    ''' 
                } 
            } 
        } 
        stage('Delete'){ 
            steps{ 
                dir('Jenkins_Assignment1'){ 
                    sh ''' 
                    git branch  -D new-feature-branch 
                    git push origin --delete  new-feature-branch 
                    ''' 
                } 
            } 
        } 
    } 
    post{ 
        success{ 
            script{ 
                withCredentials([string(credentialsId: 'SlackCred', variable: 'SLACK_TOKEN')]) {
                    def mesg ="Job ${env.JOB_NAME} ran successfully at ${env.Build_Number} on Node ${NODE_NAME}" 
                    slackSend(
                        channel: "C08FD4HNQ78", 
                        message: mesg,
                        color: 'good',
                        tokenCredentialId: 'SlackCred' )
                }
                emailext (
                body:'''<html>
<body>
<p>Hey!! Jenkins job: Assignment1 is ${BUILD_STATUS} at ${Build_Number} on Node ${NODE_NAME}. Check ${BUILD_URL} console ouptut</p>
</body>
</html>''',
                subject: "Jenkins: Pipeline Success!",
                to: 'your-mail@gmail.com',
                from: 'jenkins@example.com',
                replyTo: 'jenkins@example.com',
                mimeType: 'text/html')
            }
        }
 
        failure{
 
            script{ 
                withCredentials([string(credentialsId: 'SlackCred', variable: 'SLACK_TOKEN')])  {
                    def mesg ="Job ${env.JOB_NAME} failed at ${env.Build_Number} on Node ${NODE_NAME}" 
                    slackSend(
                        channel: "C08FD4HNQ78", 
                        message: mesg,
                        color: 'good',
                        tokenCredentialId: 'SlackCred' 
                    )
                }
                emailext (
                body:'''<html>
<body>
<p>Hey!! Jenkins job: Assignment1 is ${BUILD_STATUS} at ${Build_Number} on Node ${NODE_NAME}. Check ${BUILD_URL} console ouptut</p>
</body>
</html>''',
                subject: "Jenkins: Pipeline Failed!",
                to: 'your-mail@gmail.com',
                from: 'jenkins@example.com',
                replyTo: 'jenkins@example.com',
                mimeType: 'text/html')
            }
        } 
    } 
}
