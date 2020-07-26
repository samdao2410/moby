#!/usr/bin/env groovy

def aBadge = addEmbeddableBadgeConfiguration(id: "agent", subject: "Agent")
def iBadge = addEmbeddableBadgeConfiguration(id: "identity", subject: "Identity")
def cBadge = addEmbeddableBadgeConfiguration(id: "collector", subject: "Collector")
def dnBadge = addEmbeddableBadgeConfiguration(id: "data_analysis", subject: "Data analysis")
def exBadge = addEmbeddableBadgeConfiguration(id: "experimental", subject: "Experimental")
def rawBadge = addEmbeddableBadgeConfiguration(id: "rawwager", subject: "Raw wager")
def shBadge = addEmbeddableBadgeConfiguration(id: "scheduler", subject: "Scheduler")

pipeline {
    agent any
    parameters {
        //choice(name: 'service', choices: ['agent', 'identity', 'collector', 'dataanalysis', 'experimental', 'rawwager', 'scheduler'], description: '')
        booleanParam(name: 'agent', defaultValue: false, description: '')
        booleanParam(name: 'identity', defaultValue: false, description: '')
        booleanParam(name: 'collector', defaultValue: false, description: '')
        booleanParam(name: 'data_analysis', defaultValue: false, description: '')
        booleanParam(name: 'experimental', defaultValue: false, description: '')
        booleanParam(name: 'rawwager', defaultValue: false, description: '')
        booleanParam(name: 'scheduler', defaultValue: false, description: '')
    }
    
    environment {
        SERVICE_NAME = "${params.service}"
        REGISTRY = "registry.fb88.company"
    }
    
    stages {
        stage('Testing') {
            parallel {
                stage('Agent') {
                    when {
                        beforeAgent true
                        expression { params.agent }
                    }
                    steps {
                        script {
                            aBadge.setStatus('running')
                            try {
                                def root = tool name: 'go-1.13', type: 'go'
                                withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
                                    sh 'go test ./pkg/agent/.'
                                }
                            } catch (Exception err) {
                                aBadge.setStatus('failing')
                                aBadge.setColor('red')
                                error 'Agent test failed'
                            }
                        }
                    }
                }
                stage('Identity') {
                    when {
                        beforeAgent true
                        expression { params.identity }
                    }
                    steps {
                        script {
                            iBadge.setStatus('running')
                            try {
                                def root = tool name: 'go-1.13', type: 'go'
                                withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
                                    sh 'go test ./pkg/identity/.'
                                }
                            } catch (Exception err) {
                                iBadge.setStatus('failing')
                                iBadge.setColor('red')
                                error 'Identity test failed'
                            }
                        }
                    }
                }
                stage('Collector') {
                    when {
                        beforeAgent true
                        expression { params.collector }
                    }
                    steps {
                        script {
                            cBadge.setStatus('running')
                            try {
                                def root = tool name: 'go-1.13', type: 'go'
                                withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
                                    sh 'go test ./pkg/collector/.'
                                }
                            } catch (Exception err) {
                                cBadge.setStatus('failing')
                                cBadge.setColor('red')
                                error 'Collector test failed'
                            }
                        }
                    }
                }
                stage('dataanalysis') {
                    when {
                        beforeAgent true
                        expression { params.data_analysis }
                    }
                    steps {
                        script {
                            dnBadge.setStatus('running')
                            try {
                                def root = tool name: 'go-1.13', type: 'go'
                                withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
                                    sh 'go test ./pkg/dataanalysis/.'
                                }
                            } catch (Exception err) {
                                dnBadge.setStatus('failing')
                                dnBadge.setColor('red')
                                error 'Dataanalysis test failed'
                            }
                        }
                    }
                }
                stage('Experimental') {
                    when {
                        beforeAgent true
                        expression { params.experimental }
                    }
                    steps {
                        script {
                            exBadge.setStatus('running')
                            try {
                                def root = tool name: 'go-1.13', type: 'go'
                                withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
                                    sh 'go test ./pkg/experimental/.'
                                }
                            } catch (Exception err) {
                                exBadge.setStatus('failing')
                                exBadge.setColor('red')
                                error 'Experimental test failed'
                            }
                        }
                    }
                }
                stage('Rawwager') {
                    when {
                        beforeAgent true
                        expression { params.rawwager }
                    }
                    steps {
                        script {
                            rawBadge.setStatus('running')
                            try {
                                def root = tool name: 'go-1.13', type: 'go'
                                withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
                                    sh 'go test ./pkg/rawwager/.'
                                }
                            } catch (Exception err) {
                                rawBadge.setStatus('failing')
                                rawBadge.setColor('red')
                                error 'Rawwager test failed'
                            }
                        }
                    }
                }
                stage('Scheduler') {
                    when {
                        beforeAgent true
                        expression { params.scheduler }
                    }
                    steps {
                        script {
                            shBadge.setStatus('running')
                            try {
                                def root = tool name: 'go-1.13', type: 'go'
                                withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
                                    sh 'go test ./pkg/scheduler/.'
                                }
                            } catch (Exception err) {
                                shBadge.setStatus('failing')
                                shBadge.setColor('red')
                                error 'Scheduler test failed'
                            }
                        }
                    }
                }
            }
        }

        stage('Build') {
            parallel {
                stage('Agent') {
                    when {
                        beforeAgent true
                        expression { params.agent }
                    }
                    steps {
                        script {
                            try {
                                sh 'docker build -f cmd/agent/Dockerfile -t agent . --network=host'
                            } catch (Exception err) {
                                aBadge.setStatus('failing')
                                aBadge.setColor('red')
                                error 'Agent build failed'
                            }
                        }
                        
                    }
                }
                stage('Identity') {
                    when {
                        beforeAgent true
                        expression { params.identity }
                    }
                    steps {
                        try {
                            sh 'docker build -f cmd/identity/Dockerfile -t identity . --network=host'
                        } catch (Exception err) {
                            iBadge.setStatus('failing')
                            iBadge.setColor('red')
                            error 'Identity build failed'
                        }
                    }
                }
                stage('Collector') {
                    when {
                        beforeAgent true
                        expression { params.collector }
                    }
                    steps {
                        try {
                            sh 'docker build -f cmd/collector/Dockerfile -t collector . --network=host'
                        } catch (Exception err) {
                            cBadge.setStatus('failing')
                            cBadge.setColor('red')
                            error 'Collector build failed'
                        }
                    }
                }
                stage('dataanalysis') {
                    when {
                        beforeAgent true
                        expression { params.data_analysis }
                    }
                    steps {
                        try {
                            sh 'docker build -f cmd/dataanalysis/Dockerfile -t dataanalysis . --network=host'
                        } catch (Exception err) {
                            dnBadge.setStatus('failing')
                            dnBadge.setColor('red')
                            error 'Dataanalysis build failed'
                        }
                    }
                }
                stage('Experimental') {
                    when {
                        beforeAgent true
                        expression { params.experimental }
                    }
                    steps {
                        try {
                            sh 'docker build -f cmd/experimental/Dockerfile -t experimental . --network=host'
                        } catch (Exception err) {
                            exBadge.setStatus('failing')
                            exBadge.setColor('red')
                            error 'Experimental build failed'
                        }
                    }
                }
                stage('Rawwager') {
                    when {
                        beforeAgent true
                        expression { params.rawwager }
                    }
                    steps {
                        try {
                            sh 'docker build -f cmd/rawwager/Dockerfile -t rawwager . --network=host'
                        } catch (Exception err) {
                            rawBadge.setStatus('failing')
                            rawBadge.setColor('red')
                            error 'Rawwager build failed'
                        }
                    }
                }
                stage('Scheduler') {
                    when {
                        beforeAgent true
                        expression { params.scheduler }
                    }
                    steps {
                        try {
                            sh 'docker build -f cmd/scheduler/Dockerfile -t scheduler . --network=host'
                        } catch (Exception err) {
                            shBadge.setStatus('failing')
                            shBadge.setColor('red')
                            error 'Scheduler build failed'
                        }
                    }
                }
            }
        }

        stage('Deploy Image') {
            parallel {
                stage('Agent') {
                    when {
                        beforeAgent true
                        expression { params.agent }
                    }
                    steps {
                        script {
                            try {
                                withDockerRegistry([credentialsId: 'docker-registry', url: "http://registry.fb88.company/"]) {
                                sh 'docker tag agent ${REGISTRY}/agent:1.0'
                                sh 'docker push ${REGISTRY}/agent:1.0'
                            } catch (Exception err) {
                                aBadge.setStatus('failing')
                                aBadge.setColor('red')
                                error 'Deploy image failed'
                            }
                            aBadge.setStatus('passing')
                        }
                    }
                }
                stage('Identity') {
                    when {
                        beforeAgent true
                        expression { params.identity }
                    }
                    steps {
                        withDockerRegistry([credentialsId: 'docker-registry', url: "http://registry.fb88.company/"]) {
                            sh 'docker tag identity ${REGISTRY}/identity:1.0'
                            sh 'docker push ${REGISTRY}/identity:1.0'
                        }
                        script {
                            iBadge.setStatus('passing')
                        }
                    }
                }
                stage('Collector') {
                    when {
                        beforeAgent true
                        expression { params.collector }
                    }
                    steps {
                        withDockerRegistry([credentialsId: 'docker-registry', url: "http://registry.fb88.company/"]) {
                            sh 'docker tag collector ${REGISTRY}/collector:1.0'
                            sh 'docker push ${REGISTRY}/collector:1.0'
                        }
                        script {
                            cBadge.setStatus('passing')
                        }
                    }
                }
                stage('dataanalysis') {
                    when {
                        beforeAgent true
                        expression { params.data_analysis }
                    }
                    steps {
                        withDockerRegistry([credentialsId: 'docker-registry', url: "http://registry.fb88.company/"]) {
                            sh 'docker tag dataanalysis ${REGISTRY}/dataanalysis:1.0'
                            sh 'docker push ${REGISTRY}/dataanalysis:1.0'
                        }
                        script {
                            dnBadge.setStatus('passing')
                        }
                    }
                }
                stage('Experimental') {
                    when {
                        beforeAgent true
                        expression { params.experimental }
                    }
                    steps {
                        withDockerRegistry([credentialsId: 'docker-registry', url: "http://registry.fb88.company/"]) {
                            sh 'docker tag experimental ${REGISTRY}/experimental:1.0'
                            sh 'docker push ${REGISTRY}/experimental:1.0'
                        }
                        script {
                            exBadge.setStatus('passing')
                        }
                    }
                }
                stage('Rawwager') {
                    when {
                        beforeAgent true
                        expression { params.rawwager }
                    }
                    steps {
                        withDockerRegistry([credentialsId: 'docker-registry', url: "http://registry.fb88.company/"]) {
                            sh 'docker tag rawwager ${REGISTRY}/rawwager:1.0'
                            sh 'docker push ${REGISTRY}/rawwager:1.0'
                        }
                        script {
                            rawBadge.setStatus('passing')
                        }
                    }
                }
                stage('Scheduler') {
                    when {
                        beforeAgent true
                        expression { params.scheduler }
                    }
                    steps {
                        withDockerRegistry([credentialsId: 'docker-registry', url: "http://registry.fb88.company/"]) {
                            sh 'docker tag scheduler ${REGISTRY}/scheduler:1.0'
                            sh 'docker push ${REGISTRY}/scheduler:1.0'
                        }
                        script {
                            shBadge.setStatus('passing')
                        }
                    }
                }
            }
        }
        // stage("Testing") {      
        //     steps {
        //          script {
        //             def root = tool name: 'go-1.13', type: 'go'
        //             withEnv(["GOROOT=${root}", "PATH+GO=${root}/bin"]) {
        //                 sh 'go test ./pkg/${SERVICE_NAME}/.'
        //             }
        //          }
        //     }
        // } 
      
        // stage('Build Image') {
        //     steps {
        //         sh 'docker build -f cmd/${SERVICE_NAME}/Dockerfile -t ${SERVICE_NAME} . --network=host'
        //     }
        // }

        // stage('Deploy Image') {
        //     steps {
        //         withDockerRegistry([credentialsId: 'docker-registry', url: "http://registry.fb88.company/"]) {
        //             sh 'docker tag ${SERVICE_NAME} ${REGISTRY}/${SERVICE_NAME}:1.0'
        //             sh 'docker push ${REGISTRY}/${SERVICE_NAME}:1.0'
        //         }
        //     }
        // }

        stage('Deploy') {
            steps {
                script {
                    def remote = [:]
                    remote.name = "sev01"
                    remote.host = "192.168.5.41"
                    remote.port = 2268
                    remote.allowAnyHosts = true
                    
                    withCredentials([sshUserPrivateKey(credentialsId: 'jenkins-ssh', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'jenkins')]) {
                        remote.user = jenkins
                        remote.identityFile = identity
                        
                        //sshCommand remote: remote, command: 'docker service ls; exit'
                        sshCommand remote: remote, command: 'cd /home/support/main-deployment; docker stack deploy --with-registry-auth -c stack-main.yml -c stack-main.secret.yml -c stack-main.override.yml main; sleep 5; docker service ls; exit'
                    }
                }
            }
        }
    }
}
