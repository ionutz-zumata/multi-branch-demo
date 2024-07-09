pipeline {
    agent any    
    stages {

        stage("regular deployment") {
            when {
                anyOf {
                    branch "release/*"
                }
                not {
                    anyOf {
                        branch "release/*-beta*"
                    }
                }
            } 
            stages {
                stage("Approval") {
                    steps {
                        timeout(60) {
                            script {
                                def approvalInput = input message: "Deploy?",
                                        submitterParameter: "approver",
                                        parameters: [
                                            booleanParam(
                                                name: "DeployToBetaChannel", 
                                                defaultValue: false,
                                                description: "Deploy to beta channel?"
                                            )
                                        ]
                                env.APPROVER = "${approvalInput.approver}"
                                env.DEPLOY_TO_BETA_CHANNEL = ${approvalInput.DeployToBetaChannel}
                            }
                        }
                    }
                }

                stage("Deploy") {
                    steps {
                        echo "Signed off by: ${APPROVER}"
                        echo "Regular deployment done"
                    }
                }
            }

        }

        stage("beta deployment") {
            when {
                anyOf {
                    branch "release/*-beta*"
                    environment name: "DEPLOY_TO_BETA_CHANNEL", value: "true"
                }
            }
            stages {
                stage("Approval") {
                    steps {
                        timeout(60) {
                            script {
                                def approvalInput = input message: "Deploy?",
                                        submitterParameter: "approver",
                                        parameters: [
                                            booleanParam(name: "Yes", defaultValue: false)
                                        ]
                                env.APPROVER = "${approvalInput.approver}"
                            }
                        }
                    }
                }

                stage("Deploy") {
                    steps {
                        echo "Signed off by: ${APPROVER}"
                        echo "Beta deployment done"
                    }
                }
            }
        }
    }
}