pipeline {
    agent any

    environment {
        contra_temp = ""
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // 👉 Defino datos de entrada
                    def usuarioIngreso = input(
                        id: 'usuarioIngreso',
                        message: 'Ingrese información de usuario',
                        parameters: [
                            string(name: 'LOGIN', defaultValue: '', description: 'Nombre de usuario'),
                            string(name: 'FULL_NAME', defaultValue: '', description: 'Nombre completo'),
                            choice(name: 'DEPARTMENT', choices: ['contabilidad', 'finanzas', 'tecnología'], description: 'Seleccione uno:')
                        ]
                    )

                    // 👉 Exportar variables al entorno
                    env.LOGIN = usuarioIngreso['LOGIN']
                    env.FULL_NAME = usuarioIngreso['FULL_NAME']
                    env.DEPARTMENT = usuarioIngreso['DEPARTMENT']
                }
            }
        }

        stage('Contraseña temporal') {
            steps {
                script {
                    // 👉 Genera una contraseña temporal (8 caracteres seguros)
                    env.contra_temp = UUID.randomUUID().toString().substring(0, 8)
                    echo "Contraseña temporal: ${env.contra_temp}"
                }
            }
        }
        stage('Email') {
            steps {
                script {
                    // 👉 Enviar correo (requiere `mailutils` instalado)
                    sh """
                        echo "Hola ${FULL_NAME}, la cuenta se creó correctamente. La contraseña temporal es: ${contra_temp}" | mail -s "Creación de cuenta" usuarioprueba@jenlabo.com
                    """
                }
            }
        }

        stage('Usuario nuevo') {
            steps {
                script {
                    // 👉 Crear usuario y asignar contraseña usando `echo` directo
                    sh """
                        echo "Creando usuario ${LOGIN} en el grupo ${DEPARTMENT}..."
                        sudo useradd -m -s /bin/bash -G "${DEPARTMENT}" -c "${FULL_NAME}" "${LOGIN}" || echo "⚠️ Usuario ya existe"
                        echo "${LOGIN}:${contra_temp}" | sudo chpasswd
                        sudo chage -d 0 "${LOGIN}"
                        echo "✅ Usuario '${LOGIN}' creado con éxito"
                    """
                }
            }
        }

        
    }

    post {
        always {
            echo "✅ Pipeline terminado"
        }
    }
}
