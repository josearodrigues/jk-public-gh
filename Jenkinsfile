pipeline {
    agent any

    stages {
        stage('Clonando Repositório Git') {
            steps {
                git branch: 'main', url: 'https://github.com/josearodrigues/jk-public-gh'
            }
        }
        stage('Construindo Imagem') {
            steps {
                sh 'docker build -t webapp:${BUILD_NUMBER} .'
            }
        }
        stage('Implantando Aplicação') {
            steps {
                script {
                    // Executa o comando para verificar se o container já está rodando e parar ele, se necessário
                    sh '''
                    executando=$(docker ps | grep webapp_ctr || true)
                    if [ ! -z "$executando" ]; then
                        docker stop webapp_ctr
                    fi
                    '''
                    
                    // Executa o comando para rodar o container com a nova versão da aplicação
                    sh 'docker run --rm -d -p 3000:3000 --name webapp_ctr webapp:${BUILD_NUMBER}'
                }
            }
        }
    }
}
