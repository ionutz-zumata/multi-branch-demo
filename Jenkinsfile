pipeline {
    agent any
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 1, unit: "SECONDS")
    }
    
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
                        echo "Regular deployment done"
                    }
                }
            }

        }

        stage("beta deployment") {
            when {
                anyOf {
                    branch "release/*-beta*"
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