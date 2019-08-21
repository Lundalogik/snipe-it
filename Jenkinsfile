pipeline
{
    options
    {
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }
    agent 
    {
        label 'default' 
    }
    environment 
    {
        IMAGE = 'snipe-it:latest'
    }

    stages
    {
        stage('Docker build')
        {
            steps
            {
                script
                {
                    docker.build("$IMAGE")
                }
            }
        }

        stage('Docker push')
        {
            steps
            {
                script
                {
		    sh '''
                        set +x
                        eval \$(aws ecr get-login --region eu-west-1 --no-include-email | sed 's|https://||')
                        set -x 
                    '''
                    docker.withRegistry(env.ECRURL)
                    {
                        docker.image(IMAGE).push()
                    }
                }
            }
        }
    }
    
    post
    {
        always
        {
            sh "docker rmi $IMAGE | true"
        }
    }
} 
