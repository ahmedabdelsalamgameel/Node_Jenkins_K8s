pipeline {

    agent {label "manage-slave"}

    stages {
        stage('CI'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                {          
                    sh '''
                        docker build . -f dockerfile -t ahmedabdelsalam19/jn-node
                        docker login -u ${USERNAME} -p ${PASSWORD}
                        docker push ahmedabdelsalam19/jn-node
                    '''
                }
            }   
        }

        stage('CD'){
            steps{

                sh ''' 
                    kubectl create namespace node-ns
                    kubectl apply  -f node-k8s-files/node-dep.yml -n node-ns
                    kubectl apply  -f node-k8s-files/node-svc.yml -n node-ns
                '''
            }
        }
    }
}