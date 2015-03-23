# Prueba de Recursos
La Prueba de Recursos permite a la red validar acciones o servicios usando un mecanismo matemáticamente verificable.

Este proceso mide la habilidad de los diferentes Vaults a la hora de gestionar los diferentes chunks de datos, y esto depende de los siguientes criterios:

* Velocidad de CPU
* Disponibilidad del ancho de banda
* Espacio de disco
* Tiempo on-line

Esto permite probar que se es una entidad útil, medible y verificable. La prueba de Recursos es un eficiente mecanismo sin gastos de transacción.

La prueba de recurso usa un mecanismo similar a la [prueba de conocimiento cero](http://es.wikipedia.org/wiki/Prueba_de_conocimiento_cero). En este caso el mecanismo de comprobación no requiere conocer el contenido de ningún dato que se va a comprobar pero requiere saber que el dato existe y que es preciso.

La Prueba de Recursos sigue una serie de pasos.

1. Un grupo de control compuesto de Vaults crea una cadena aleatoria
2. Esta cadena aleatoria se envía encriptada a todos los poseedores de los datos
3. Los gestores de datos (Data Holders) añaden esta cadena a los datos originales y crean un Hash con el resultado
4. El resultado se recoge y desencripta, por el grupo de control, y se compara
5. Si algún Vault devuelve un resultado diferente se considera comprometido y se le baja de nivel

Este mecanismo se activa en las peticiones Get y durante las transferencia de cuentas. Es no determinista y aleatorio de usuario a usuario. Se considera seguro y el uso de una prueba de cero conocimiento no se realiza para ocultar el contenido (cualquiera puede pedir cualquier dato) sino para asegurar que no se transferirán datos contaminados.

Aquí hay un resumen del proceso de Prueba de Recursos.

![Proof of resource figure](./img/por-diagram.png)
