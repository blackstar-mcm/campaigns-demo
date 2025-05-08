stage('run container') {
    steps {
        sh '''
            # Eliminamos el contenedor existente si ya existe
            if [ "$(docker ps -aq -f name=campaign-demo-server)" ]; then
                docker rm -f campaign-demo-server
            fi

            # Ejecutamos el nuevo contenedor
            docker run -d --name campaign-demo-server --label campaign-demo-server -p 8081:8081 miichellecame/campaign-demo:v1
        '''
    }
}

