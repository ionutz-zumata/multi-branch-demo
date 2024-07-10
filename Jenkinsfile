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
                                            [
                                                $class: 'hudson.model.ChoiceParameterDefinition',
                                                choices: 'Approved\nNot Approved',
                                                name: 'isApproved',
                                                description: 'This can be checked by' +
                                                    ' users with approval permission'
                                            ],
                                            booleanParam(
                                                name: "Deploy To Beta Channel", 
                                                defaultValue: false,
                                            )
                                        ]
                                env.APPROVER = "${approvalInput.approver}"
                                env.DEPLOY_TO_BETA_CHANNEL = "${approvalInput['Deploy To Beta Channel']}"
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