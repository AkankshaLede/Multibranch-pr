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

// // pipeline {
// //     agent any

// //     stages {
// //         stage('Check for CFG Changes') {
// //             steps {
// //                 script {
// //                     def changedFiles = sh(script: 'git diff --name-only origin/main...HEAD', returnStdout: true).trim()
// //                     if (changedFiles) {
// //                         def cfgFilesChanged = changedFiles.split('\n').any { it.endsWith('.cfg') }
// //                         if (cfgFilesChanged) {
// //                             echo "Changes detected in .cfg files on branch: ${GIT_BRANCH}"
// //                             env.CFG_CHANGES_DETECTED = true
// //                         } else {
// //                             echo "No changes detected in .cfg files on branch: ${GIT_BRANCH}"
// //                             env.CFG_CHANGES_DETECTED = false
// //                         }
// //                     } else {
// //                         echo "No changes found compared to origin/main on branch: ${GIT_BRANCH}"
// //                         env.CFG_CHANGES_DETECTED = false
// //                     }
// //                 }
// //             }
// //         }

// //         stage('Create Pull Request (if CFG changed)') {
// //             when {
// //                 environment name: 'CFG_CHANGES_DETECTED', value: 'true'
// //             }
// //             steps {
// //                 withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
// //                     sh '''
// //                         unset GITHUB_TOKEN
// //                         echo "$GITHUB_TOKEN" | gh auth login --with-token
// //                         gh pr create --title "Automated PR: Changes in CFG files on ${GIT_BRANCH}" --body "This PR was automatically created due to changes detected in .cfg files on the ${GIT_BRANCH} branch." --base main --head ${GIT_BRANCH}
// //                     '''
// //                 }
// //             }
// //         }

// //         // You can add more stages here for building, testing, etc.
// //     }
// // }


pipeline {
    agent any

    stages {
        stage('Check for CFG Changes and Create PR') {
            when {
                branch 'automation1'
            }
            steps {
                script {
                    checkout scm // Checkout the specific branch

                    def changedFiles = sh(script: 'git diff --name-only origin/main...HEAD -- *.cfg', returnStdout: true).trim()

                    if (changedFiles) {
                        echo "Detected changes in .cfg files in automation1:"
                        echo "${changedFiles}"

                        // Check if a PR already exists (requires GitHub CLI or API interaction)
                        // This is a simplified example using a placeholder function
                        if (!pullRequestExists('automation1', 'main')) {
                            echo "Creating pull request from automation1 to main"
                            createPullRequest('automation1', 'main', 'Automated PR due to .cfg file changes', 'Changes detected in .cfg files.')
                        } else {
                            echo "Pull request from automation1 to main already exists."
                        }
                    } else {
                        echo "No changes detected in .cfg files in automation1."
                    }
                }
            }
        }

        stage('Check for CFG Changes and Create PR') {
            when {
                branch 'automation2'
            }
            steps {
                script {
                    checkout scm // Checkout the specific branch

                    def changedFiles = sh(script: 'git diff --name-only origin/main...HEAD -- *.cfg', returnStdout: true).trim()

                    if (changedFiles) {
                        echo "Detected changes in .cfg files in automation2:"
                        echo "${changedFiles}"

                        // Check if a PR already exists (requires GitHub CLI or API interaction)
                        // This is a simplified example using a placeholder function
                        if (!pullRequestExists('automation2', 'main')) {
                            echo "Creating pull request from automation2 to main"
                            createPullRequest('automation2', 'main', 'Automated PR due to .cfg file changes', 'Changes detected in .cfg files.')
                        } else {
                            echo "Pull request from automation2 to main already exists."
                        }
                    } else {
                        echo "No changes detected in .cfg files in automation2."
                    }
                }
            }
        }
    }
}

// Placeholder functions - you'll need to implement these
def pullRequestExists(String sourceBranch, String targetBranch) {
    // Implementation to check if a pull request already exists
    // This will likely involve using the GitHub API or CLI
    // For example, using the 'gh' CLI:
    // sh(script: "gh pr list --base ${targetBranch} --head ${sourceBranch} --state open", returnStatus: true) == 0
    echo "Placeholder: Checking if PR exists from ${sourceBranch} to ${targetBranch}"
    return false // Replace with actual logic
}

def createPullRequest(String sourceBranch, String targetBranch, String title, String body) {
    // Implementation to create a pull request
    // This will likely involve using the GitHub API or CLI
    // For example, using the 'gh' CLI:
    // sh(script: "gh pr create --base ${targetBranch} --head ${sourceBranch} --title \"${title}\" --body \"${body}\"", returnStatus: true)
    echo "Placeholder: Creating PR from ${sourceBranch} to ${targetBranch} with title: ${title}"
}