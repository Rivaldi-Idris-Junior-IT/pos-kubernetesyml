def builderDocker
def CommitHash

pipeline {
    agent any

    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value for testing')        
        choice(name: 'CICD', choices: ['CI', 'CICD'], description: 'Pick something')        
        choice(name: 'Mode', choices: ['master','development', 'production'], description: 'Pili mode push')
    }

    stages {
        
        stage('Build Docker Images') {
            steps {
                script {
                    if (params.Mode == GIT_BRANCH) {                        
                        sh 'echo Validasi branch berhasil'
                    } else if (params.Mode != GIT_BRANCH) {
                        currentBuild.result = 'ABORTED'
                        error('Validasi branch gagal …')
                    }
                }
            }
        }

        stage('Run Testing') {
            when {
                expression {
                    params.RUNTEST
                }
            }
            steps {
                sh 'RUNTES PASSED'
            }
        }
        
        stage("Execute yml file"){
            when {
                expression {
                    params.CICD == 'CICD'
                }
            }
            
            steps {
                script{
                    if (params.Deploy == 'deployement') {
                        sshPublisher(
                            publishers: [
                                sshPublisherDesc(
                                    configName: 'k8s-controll',
                                    verbose: false,
                                    transfers: [
                                        sshTransfer(
                                            execCommand: 'kubectl delete -n backend-frontend deployment.apps/pos-frontend; kubectl delete -n backend-frontend deployment.apps/pos-backend; kubectl delete -n backend-frontend service/pos-frontend; kubectl delete -n backend-frontend service/pos-backend; kubectl apply -f backend-dep.yml',
                                            execTimeout: 250000,
                                        )
                                    ]
                                )
                            ]
                        )
                    }
                }
                echo 'apply succes.'
            }
        }

        
    }
}