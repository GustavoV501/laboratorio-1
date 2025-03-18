Descripción:
Este proyecto desarrolla un pipeline declarativo en Jenkins que nos permite crear usuarios dentro de un sistema linux. 
Permite a un operador de seguridad crear usuarios, generar contraseñas temporales y enviarlas por correo electrónico, todo a través de un pipeline de Jenkins.

Herramientas: Jenkins, terminal linux, token de github.

Funcionamiento: Primero se solicita al operador de seguridad ingresar el nombre de usuario, nombre completo y seleccionar el departamento.

Lo siguiente es generar una contraseña temporal aleatoria, el nuevo usuario deberá cambiar su contraseña en su primer inicio de sesión.

Por comandos linux se genera un nuevo usuario, se le asigna la contraseña temporal y se pide cambiar la contraseña.

Se envía un correo electrónico al usuario con la contraseña temporal generada.

Error en Jenkins: Tuve un problema con el código en la funcionalidad Crear un usuario, no pude resolver el problema, intente varias formas de resolver ya sea cambiando useradd, sudo -S chpasswd entre otras cosas pero obtuve el mismso resultado,
el código paso por varios cambios por el mismo error, cambie varias sentencias pero aún asi el error persiste.
