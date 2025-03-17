pipeline {
    agent any

    environment {
        TEMP_PASSWORD = ""
    }

    stages {
        stage('Build') {
            steps {
                script {
                   
                    def usuarioIngreso = input(
                        id: 'usuarioIngreso',
                        message: 'Ingrese información de usuario',
                        parameters: [
                            string(name: 'LOGIN', defaultValue: '', description: 'Unique identifier (name.lastname)'),
                            string(name: 'FULL_NAME', defaultValue: '', description: 'User full name'),
                        ]
                    )
        
                    env.LOGIN = userInput.LOGIN
                    env.FULL_NAME = userInput.FULL_NAME

                }
            }
        }

        stage('Contraseña temporal') {
            steps {
                script {
                    // Generar una contraseña temporal
                    TEMP_PASSWORD = UUID.randomUUID().toString().substring(0, 8)
                    echo "Generated temporary password: ${TEMP_PASSWORD}"
                }
            }
        }

        stage('Usuario nuevo') {
            steps {
                script {
                    // Crear el usuario 
                    sh """
                        sudo useradd -m -s /bin/bash ${LOGIN} -c "${FULL_NAME}" 
                        echo "${LOGIN}:${TEMP_PASSWORD}" | sudo chpasswd
                        sudo chage -d 0 ${LOGIN}  
                    """
                }
            }
        }

        stage('Email') {
            steps {
                script {
                    sh """
                        echo "Hola ${FULL_NAME}, tu cuenta ha sido creada. Tu contraseña temporal es: ${TEMP_PASSWORD}" | mail -s "Creación de cuenta" usuarioprueba@jenlabo.com
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
