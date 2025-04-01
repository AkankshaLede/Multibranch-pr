pipeline {
    agent any

    stages {
        stage('Check for CFG Changes') {
            steps {
                script {
                    def changedFiles = sh(script: 'git diff --name-only HEAD~1', returnStdout: true).trim()
                    if (changedFiles) {
                        def cfgFilesChanged = changedFiles.split('\n').any { it.endsWith('.cfg') }
                        if (cfgFilesChanged) {
                            echo "Changes detected in .cfg files."
                            env.CFG_CHANGES_DETECTED = true
                        } else {
                            echo "No changes detected in .cfg files."
                            env.CFG_CHANGES_DETECTED = false
                        }
                    } else {
                        echo "No changes found in the last commit."
                        env.CFG_CHANGES_DETECTED = false
                    }
                }
            }
        }

        stage('Create Pull Request (if CFG changed)') {
            when {
                environment name: 'CFG_CHANGES_DETECTED', value: 'true'
            }
            steps {
                withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        unset GITHUB_TOKEN
                        echo "$GITHUB_TOKEN" | gh auth login --with-token
                        gh pr create --title "Automated PR: Changes in CFG files" --body "This PR was automatically created due to changes detected in .cfg files." --base automation --head ${GIT_BRANCH}
                    '''
                }
            }
        }

        // You can add more stages here for building, testing, etc.
    }
}