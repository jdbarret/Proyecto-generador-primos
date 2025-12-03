# Proyecto final Sistemas Distribuidos - Generador de numeros primos 

# Nombres - Códigos
Laura Vanessa Hernández García - 160004630
Jose Daniel Barreto Aguilera - 160004622

# Instrucciones de uso y despliegue

Para desplegar el generador de numeros primos, primero ingresar a:
https://killercoda.com/playgrounds/scenario/kubernetes   
(Se requiere inicio de sesión)

Se abrirá una terminal de comandos, donde inicialmente se obtendrá el repositorio del proyecto

# Ejecutar el siguiente comando
git clone https://github.com/jdbarret/Proyecto-generador-primos.git 

# Dirigirse al directorio
cd Proyecto_final_Distribuidos

# Ahora se ejecutan una serie de comandos, que para mayor facilidad se ejecutan en un script

chmod +x script-arranque.sh

./script-arranque.sh

Verificar que todos digan running y estamos listo para probar el sistema.

Esperar 100 segundos y ejecutar el siguiente comando, para verificar que todos tengas 
status: "Running"

kubectl get all -n prime-system


# Para probar el sistema debemos hacer peticiones a la api

# Crear un request
Para crear un request es necesario crear la siguiente petición donde podemos definir la cantidad de números primos
a generar y los dígitos que deseamos en cada número

"NOTA IMPORTANTE, SI ABRE EL README DESDE GITHUB LOS \ NO SE VISUALIZAN CORRECTAMENTE, DEBE IR DIRECTAMENTE AL ARCHIVO"

"
RESPONSE=$(curl -s -X POST http://localhost:30080/api/new \
  -H "Content-Type: application/json" \
  -d '{"quantity": 5, "digits": 12}')

"
Luego al enviar esta petición se creará un request_id.

Para extraerlo

REQUEST_ID=$(echo $RESPONSE | grep -o '"request_id":"[^"]*"' | cut -d'"' -f4)

echo "Request ID: $REQUEST_ID"

Se obtendrá una salida como esta, la cual usaremos para request_id para las siguientes peticiones:
a7dcb850-c522-40bd-93c9-4536a39fca08

curl http://localhost:30080/api/status/{request_id}

Luego ya podremos ver los resultados del generador de números primos, donde también especifiquemos el request_id.

curl http://localhost:30080/api/result/{request_id}

Al ejecutar este comando, en la consola se mostrarán los números primos que han sido solicitados en la petición inicial,
este es un ejemplo de visualización si todo fue exitoso:
{"request_id":"a7dcb850-c522-40bd-93c9-4536a39fca08","quantity":5,"generated_count":5,"status":"completed",
"prime_numbers":["913060254787","101171971781","156908175719","623459046827","356057906851"]}






