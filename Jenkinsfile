// pipeline {
//     agent any
//     environment {
//         // Generate a timestamp for tagging
//         TIMESTAMP = "${new Date().format('yyyyMMdd-HHmm')}"
//         TAG_NAME = "cfg-change-${TIMESTAMP}" // Define TAG_NAME in the environment
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

//         stage('Create GitHub Release (if CFG changed)') {
//             when {
//                 environment name: 'CFG_CHANGES_DETECTED', value: 'true'
//             }
//             steps {
//                 withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
//                     sh '''
//                         unset GITHUB_TOKEN
//                         echo "$GITHUB_TOKEN" | gh auth login --with-token
//                         gh release create "$TAG_NAME" \
//                             --title "CFG Changes Release - $TAG_NAME" \
//                             --notes "Automated release created due to .cfg changes."
//                     '''
//                 }
//             }
//         }
//     }
// }


// pipeline {
//     agent any
//     environment {
//         // Generate a timestamp for tagging
//         TIMESTAMP = "${new Date().format('yyyyMMdd-HHmm')}"
//         TAG_NAME = "cfg-change-${TIMESTAMP}" // Define TAG_NAME in the environment
//     }
//
//     stages {
//         stage('Check for CFG Changes') {
//             steps {
//                 script {
//                     def changedFiles = sh(script: 'git diff --name-only HEAD~1', returnStdout: true).trim()
//                     if (changedFiles) {
//                         def cfgFilesChanged = changedFiles.split('\n').any { it.endsWith('.cfg') }
//                         if (cfgFilesChanged) {
//                             echo "Changes detected in .cfg files."
//                             env.CFG_CHANGES_DETECTED = "true"
//                         } else {
//                             echo "No changes detected in .cfg files."
//                             env.CFG_CHANGES_DETECTED = "false"
//                         }
//                     } else {
//                         echo "No changes found in the last commit."
//                         env.CFG_CHANGES_DETECTED = "false"
//                     }
//                 }
//             }
//         }
//
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
//                                      --base main --head ${GIT_BRANCH} || echo "PR might already exist. Skipping creation."
//                     '''
//                 }
//             }
//         }
//
//         stage('Create GitHub Release (if CFG changed)') {
//             when {
//                 environment name: 'CFG_CHANGES_DETECTED', value: 'true'
//             }
//             steps {
//                 withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
//                     sh '''
//                         unset GITHUB_TOKEN
//                         echo "$GITHUB_TOKEN" | gh auth login --with-token
//                         gh release create "$TAG_NAME" \
//                             --title "CFG Changes Release - $TAG_NAME" \
//                             --notes "Automated release created due to .cfg changes." || echo "Release might already exist. Skipping creation."
//                     '''
//                 }
//             }
//         }
//     }
// }





// ----------------------------------------------------
// Import UUID for generating unique temporary file names
import java.util.UUID

pipeline {
    agent any

    environment {
        // Generate a timestamp for tagging purposes (e.g., '20230612-1430')
        TIMESTAMP = "${new Date().format('yyyyMMdd-HHmm')}"
        // Define the tag name using the generated timestamp
        TAG_NAME = "cfg-change-${TIMESTAMP}"
    }

    stages {
        stage('Check for CFG Changes') {
            steps {
                script { // This script block is correctly placed for Groovy logic
                    // Get a list of files changed in the last commit (compared to HEAD~1)
                    def changedFiles = sh(script: 'git diff --name-only HEAD~1', returnStdout: true).trim()

                    if (changedFiles) {
                        // Split the string of changed files into a list and check if any end with '.cfg'
                        def cfgFilesChanged = changedFiles.split('\n').any { it.endsWith('.cfg') }
                        if (cfgFilesChanged) {
                            echo "Changes detected in .cfg files."
                            env.CFG_CHANGES_DETECTED = true // Set environment variable to true
                        } else {
                            echo "No changes detected in .cfg files."
                            env.CFG_CHANGES_DETECTED = false // Set environment variable to false
                        }
                    } else {
                        echo "No changes found in the last commit."
                        env.CFG_CHANGES_DETECTED = false // Set environment variable to false
                    }
                }
            }
        }

        stage('Create Pull Request (if CFG changed)') {
            // This stage will only run if CFG_CHANGES_DETECTED environment variable is 'true'
            when {
                environment name: 'CFG_CHANGES_DETECTED', value: 'true'
            }
            steps {
                // Use withChecks for GitHub status updates specific to PR creation
                withChecks(name: 'Create Pull Request') {
                    // Access GitHub Personal Access Token securely from Jenkins Credentials
                    withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
                        script { // <--- Added this script block here
                            def commandStatus = 0   // Variable to hold the exit status of the gh commands
                            def commandOutput = ""  // Variable to hold the full output of the gh commands
                            def outputFileName = "gh_pr_output_${UUID.randomUUID().toString()}.txt" // Unique temp file name

                            try {
                                // Clear or create the temporary file before starting
                                sh "> ${outputFileName}"

                                // Execute gh commands in a single shell script.
                                // We use 'set +e' to ensure the script continues to capture errors,
                                // but we explicitly check exit codes and exit early on critical failures (like auth).
                                commandStatus = sh(script: """
                                    # Do not exit immediately on error, allow capturing output
                                    set +e

                                    # Redirect all subsequent stdout/stderr to the temporary file
                                    exec >${outputFileName} 2>&1

                                    # --- 1. Authenticate with GitHub CLI ---
                                    unset GITHUB_TOKEN # Ensure GITHUB_TOKEN is cleared from environment before setting
                                    echo "$GITHUB_TOKEN" | gh auth login --with-token
                                    LOGIN_STATUS=\$?
                                    if [ \$LOGIN_STATUS -ne 0 ]; then
                                        echo "Error: 'gh auth login' failed with status \$LOGIN_STATUS. Check token or network."
                                        exit \$LOGIN_STATUS # Exit the shell script immediately on auth failure
                                    fi
                                    echo "gh auth login successful."

                                    # --- 2. Create Pull Request ---
                                    gh pr create --title "Automated PR: Changes in CFG files" \\
                                                 --body "This PR was automatically created due to changes detected in .cfg files." \\
                                                 --base main --head ${GIT_BRANCH}
                                    PR_CREATE_STATUS=\$?
                                    if [ \$PR_CREATE_STATUS -ne 0 ]; then
                                        echo "Error: 'gh pr create' failed with status \$PR_CREATE_STATUS."
                                        exit \$PR_CREATE_STATUS # Exit with the PR creation status
                                    fi
                                    echo "Pull Request created successfully."

                                    # If all commands succeeded, exit with 0
                                    exit 0
                                """, returnStatus: true) // Capture the final exit status of the shell script

                                // After the shell command completes, read the content of the temporary file
                                commandOutput = readFile outputFileName
                                
                                // Clean up the temporary file to avoid clutter in the workspace.
                                sh "rm -f " + outputFileName

                                echo "GitHub PR command raw output:\n${commandOutput}"
                                echo "GitHub PR command exit status: ${commandStatus}"

                                // If the commandStatus is non-zero, it indicates a failure
                                if (commandStatus != 0) {
                                    // Provide a specific error message including the captured output
                                    error("GitHub Pull Request creation failed. Details:\n${commandOutput}")
                                } else {
                                    echo "Pull Request created successfully for branch: ${GIT_BRANCH}"
                                }
                            } catch (err) {
                                // Catch any unexpected Groovy errors within this block
                                error("An unexpected error occurred during GitHub PR creation: ${err}")
                            }
                        } // <--- Closed script block here
                    }
                }
            }
        }

        stage('Create GitHub Release (if CFG changed)') {
            // This stage will only run if CFG_CHANGES_DETECTED environment variable is 'true'
            when {
                environment name: 'CFG_CHANGES_DETECTED', value: 'true'
            }
            steps {
                // Use withChecks for GitHub status updates specific to Release creation
                withChecks(name: 'Create GitHub Release') {
                    // Access GitHub Personal Access Token securely from Jenkins Credentials
                    withCredentials([string(credentialsId: 'github-pat-token', variable: 'GITHUB_TOKEN')]) {
                        script { // <--- Added this script block here
                            def commandStatus = 0   // Variable to hold the exit status of the gh commands
                            def commandOutput = ""  // Variable to hold the full output of the gh commands
                            def outputFileName = "gh_release_output_${UUID.randomUUID().toString()}.txt" // Unique temp file name

                            try {
                                // Clear or create the temporary file before starting
                                sh "> ${outputFileName}"

                                // Execute gh commands in a single shell script.
                                // Similar logic as PR creation: ensure authentication, then attempt release.
                                commandStatus = sh(script: """
                                    set +e # Do not exit immediately on error

                                    exec >${outputFileName} 2>&1

                                    # --- 1. Authenticate with GitHub CLI ---
                                    unset GITHUB_TOKEN
                                    echo "$GITHUB_TOKEN" | gh auth login --with-token
                                    LOGIN_STATUS=\$?
                                    if [ \$LOGIN_STATUS -ne 0 ]; then
                                        echo "Error: 'gh auth login' failed with status \$LOGIN_STATUS. Check token or network."
                                        exit \$LOGIN_STATUS # Exit the shell script immediately on auth failure
                                    fi
                                    echo "gh auth login successful."

                                    # --- 2. Create GitHub Release ---
                                    gh release create "$TAG_NAME" \\
                                        --title "CFG Changes Release - $TAG_NAME" \\
                                        --notes "Automated release created due to .cfg changes."
                                    RELEASE_CREATE_STATUS=\$?
                                    if [ \$RELEASE_CREATE_STATUS -ne 0 ]; then
                                        echo "Error: 'gh release create' failed with status \$RELEASE_CREATE_STATUS."
                                        exit \$RELEASE_CREATE_STATUS # Exit with the release creation status
                                    fi
                                    echo "GitHub Release created successfully."

                                    # If all commands succeeded, exit with 0
                                    exit 0
                                """, returnStatus: true) // Capture the final exit status of the shell script

                                // After the shell command completes, read the content of the temporary file
                                commandOutput = readFile outputFileName
                                
                                // Clean up the temporary file
                                sh "rm -f " + outputFileName

                                echo "GitHub Release command raw output:\n${commandOutput}"
                                echo "GitHub Release command exit status: ${commandStatus}"

                                // If the commandStatus is non-zero, it indicates a failure
                                if (commandStatus != 0) {
                                    // Provide a specific error message including the captured output
                                    error("GitHub Release creation failed. Details:\n${commandOutput}")
                                } else {
                                    echo "GitHub Release created successfully: ${TAG_NAME}"
                                }
                            } catch (err) {
                                // Catch any unexpected Groovy errors within this block
                                error("An unexpected error occurred during GitHub Release creation: ${err}")
                            }
                        } // <--- Closed script block here
                    }
                }
            }
        }
    }
}
