// pipeline {
//     agent any

//     stages {
//         stage('Check for CFG Changes') {
//             steps {
//                 script {
//                     def changedFiles = sh(script: 'git diff --name-only HEAD~1', returnStdout: true).trim()
//                     if (changedFiles) {
//                         def cfgFilesChanged = changedFiles.split('\n').any { it.endsWith('.cfg') }
//                         if (cfgFilesChanged) {
//                             echo "Changes detected in .cfg files."
//                             env.CFG_CHANGES_DETECTED = true
//                         } else {
//                             echo "No changes detected in .cfg files."
//                             env.CFG_CHANGES_DETECTED = false
//                         }
//                     } else {
//                         echo "No changes found in the last commit."
//                         env.CFG_CHANGES_DETECTED = false
//                     }
//                 }
//             }
//         }

//         stage('Create Pull Request (if CFG changed)') {
//             when {
//                 environment name: 'CFG_CHANGES_DETECTED', value: 'true'
//             }
//             steps {
//                 withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
//                     sh '''
//                         unset GITHUB_TOKEN
//                         echo "$GITHUB_TOKEN" | gh auth login --with-token
//                         gh pr create --title "Automated PR: Changes in CFG files" --body "This PR was automatically created due to changes detected in .cfg files." --base main --head ${GIT_BRANCH}
//                     '''
//                 }
//             }
//         }

//         // You can add more stages here for building, testing, etc.
//     }
// }




// -------------------------------------------------------------------------------------------------------------
// pipeline {
//     agent any

//     environment {
//         // Generate a timestamp for tagging (e.g., cfg-change-20250407-1750)
//         TIMESTAMP = "${new Date().format('yyyyMMdd-HHmm')}"
//     }

//     stages {
//         stage('Check for CFG Changes') {
//             steps {
//                 script {
//                     def changedFiles = sh(script: 'git diff --name-only HEAD~1', returnStdout: true).trim()
//                     if (changedFiles) {
//                         def cfgFilesChanged = changedFiles.split('\n').any { it.endsWith('.cfg') }
//                         if (cfgFilesChanged) {
//                             echo "Changes detected in .cfg files."
//                             env.CFG_CHANGES_DETECTED = 'true'
//                         } else {
//                             echo "No changes detected in .cfg files."
//                             env.CFG_CHANGES_DETECTED = 'false'
//                         }
//                     } else {
//                         echo "No changes found in the last commit."
//                         env.CFG_CHANGES_DETECTED = 'false'
//                     }
//                 }
//             }
//         }

//         stage('Create Pull Request (if CFG changed)') {
//             when {
//                 environment name: 'CFG_CHANGES_DETECTED', value: 'true'
//             }
//             steps {
//                 withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
//                     sh '''
//                         unset GITHUB_TOKEN
//                         echo "$GITHUB_TOKEN" | gh auth login --with-token
//                         gh pr create --title "Automated PR: Changes in CFG files" \
//                                      --body "This PR was automatically created due to changes detected in .cfg files." \
//                                      --base main --head ${GIT_BRANCH}
//                     '''
//                 }
//             }
//         }

//         stage('Create Tag (if CFG changed)') {
//             when {
//                 environment name: 'CFG_CHANGES_DETECTED', value: 'true'
//             }
//             steps {
//                 script {
//                     env.TAG_NAME = "cfg-change-${env.TIMESTAMP}"
//                 }
//                 sh '''
//                     git tag ${TAG_NAME}
//                     git push origin ${TAG_NAME}
//                 '''
//             }
//         }

//         stage('Create GitHub Release (if CFG changed)') {
//             when {
//                 environment name: 'CFG_CHANGES_DETECTED', value: 'true'
//             }
//             steps {
//                 withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
//                     sh '''
//                         echo "$GITHUB_TOKEN" | gh auth login --with-token
//                         gh release create ${TAG_NAME} \
//                             --title "CFG Changes Release - ${TAG_NAME}" \
//                             --notes "Automated release created due to .cfg changes."
//                     '''
//                 }
//             }
//         }
//     }
// }


// -------------------------------------------------------------------------------------------------------------

pipeline {
    agent any

    environment {
        // Generate a timestamp for tagging (e.g., cfg-change-20250407-1750)
        TIMESTAMP = "${new Date().format('yyyyMMdd-HHmm')}"
    }

    stages {
        stage('Check for CFG Changes') {
            steps {
                script {
                    def changedFiles = sh(script: 'git diff --name-only HEAD~1', returnStdout: true).trim()
                    if (changedFiles) {
                        def cfgFilesChanged = changedFiles.split('\n').any { it.endsWith('.cfg') }
                        if (cfgFilesChanged) {
                            echo "Changes detected in .cfg files."
                            env.CFG_CHANGES_DETECTED = 'true'
                        } else {
                            echo "No changes detected in .cfg files."
                            env.CFG_CHANGES_DETECTED = 'false'
                        }
                    } else {
                        echo "No changes found in the last commit."
                        env.CFG_CHANGES_DETECTED = 'false'
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
                        gh pr create --title "Automated PR: Changes in CFG files" \
                                     --body "This PR was automatically created due to changes detected in .cfg files." \
                                     --base main --head automation1
                    '''
                }
            }
        }

        stage('Create Tag (if CFG changed)') {
            when {
                environment name: 'CFG_CHANGES_DETECTED', value: 'true'
            }
            steps {
                script {
                    env.TAG_NAME = "cfg-change-${env.TIMESTAMP}"
                }
                withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        unset GITHUB_TOKEN
                        git config --global user.email "jenkins@example.com"
                        git config --global user.name "Jenkins"
                        git tag "$TAG_NAME"
                        git push https://$GITHUB_TOKEN@github.com/AkankshaLede/Multibranch-pr.git "$TAG_NAME"
                    '''
                }
            }
        }

        stage('Create GitHub Release (if CFG changed)') {
            when {
                environment name: 'CFG_CHANGES_DETECTED', value: 'true'
            }
            steps {
                withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        unset GITHUB_TOKEN
                        echo "$GITHUB_TOKEN" | gh auth login --with-token
                        gh release create "$TAG_NAME" \
                            --title "CFG Changes Release - $TAG_NAME" \
                            --notes "Automated release created due to .cfg changes."
                    '''
                }
            }
        }
    }
}
// -------------------------------------------------------------------------------------------------------------