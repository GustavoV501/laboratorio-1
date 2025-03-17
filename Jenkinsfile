pipeline {
    agent any

    environment {
        contra_temp = ""
    }

    stages {
        stage('Build') {
            steps {
                script {
                   //Defino datos de entrada
                    def usuarioIngreso = input(
                        id: 'usuarioIngreso',
                        message: 'Ingrese información de usuario',
                        parameters: [
                            string(name: 'LOGIN', defaultValue: '', description: 'Identificacion (name.lastname)'),
                            string(name: 'FULL_NAME', defaultValue: '', description: 'name')
                        ]
                    )
        
                    env.LOGIN = usuarioIngreso.LOGIN
                    env.FULL_NAME = usuarioIngreso.FULL_NAME

                }
            }
        }

        stage('Contraseña temporal') {
            steps {
                script {
                    // Genera una contraseña 
                    contra_temp = UUID.randomUUID().toString().substring(0, 9)
                    echo "Contraseña temporal: ${contra_temp}"
                }
            }
        }

        stage('Usuario nuevo') {
            steps {
                script {
                    // Crear el usuario 
                    sh """
                        sudo useradd -m -s /bin/bash ${LOGIN} -c "${FULL_NAME}" 
                        echo "${LOGIN}:${contra_temp}" | sudo chpasswd
                        sudo chage -d 0 ${LOGIN}  
                    """
                }
            }
        }

        stage('Email') {
            steps {
                script {
                    sh """
                        echo "Hola ${FULL_NAME}, la cuenta se creo correctamente la contraseña temporal es: ${contra_temp}"  mail -s "Creación de cuenta" usuarioprueba@jenlabo.com
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Deploying nuevo pipeline"
        }
    }
}
