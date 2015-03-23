#Safecoins

Los safecoins pueden ser ganados, negociados o comprados. El valor de los safecoins vendrá determinado por el mercado, a través de la presión de la oferta y la demanda. 

## Precio de mercado
En número de safecoin en circulación aumentará según el uso de la red. Casi todos los poseedores iniciales de safecoin serán Farmers (granjeros) que con su suministro de recursos crearán tanto liquidez como distribución de riqueza. Se prevee que casi todos los usuarios poseerán al menos unos safecoins en su wallet.

Los usuarios pueden negociar sus safecoin por servicios en la red, o por liquidez (u otra moneda digital) usando un exchange. 
El ratio de los safecoin guardados en los wallet y aquellos gastados por los Farmers generarán un precio. Este precio será el valor de mercado de los safecoin.

## Recursos y monedas

Los safecoin se usan para acceder a servicios dentro de la red SAFE. Esto incentiva la reutilización constante cuyo resultado es el aumento de la demanda de un recurso finito. Esto hace que el valor de las safecoins aumente con el tiempo. Mientras que las monedas aumentan de valor, la cantidad de servicios en la red (recursos) que cada safecoin pueda comprar también aumenta. Esto se muestra en la fig. 2.

![](safecoin resources.png)

## Velocidad de farming 

Farming es un proceso en el cual los usuarios suministran recursos (espacio almacenamiento, CPU, ancho de banda) a la red.

Como muestra la figura 1, el algoritmo de ganancia de los safecoin es una curva sigmoide, en la que todos los Vault empiezan a ganar lentamente y donde su tasa aumenta a medida que los farmers alcanzan el promedio de la red. La tasa de ganancia también tiene en cuenta el rango de cada Vault, un proceso en el cual la red puntúa la utilidad de cada nodo entre 0 (el peor) y 1 (el mejor). La tasa del farming de safecoin es, en última instancia, el resultado de su media en la red y el balance entre oferta y demanda, multiplicado por el rango del Vault. La tasa comenzará a descender para los que se situen en el 20% superior del total de la media. Esto evitará la existencia de grandes Vault que podría llevar a la centralización del proceso de Farming. Los safecoin se asignan por la red y se paga cuando un nodo entrega correctamente un dato (GETS) y no cuando se guarda (PUTS).


![](safecoin farming speed.png)

La red incrementa automáticamente la recompensa cuando aumenta la demanda de almacenamiento y la reduce cuando esta es abundante. Los datos se distribuyen de manera uniforme en la red y por lo tanto, los farmers
que buscan maximizar sus ganancias puede hacerlo mediante la ejecución de varios nodos con un rendimiento medio en lugar de disponer un solo nodo de alto rendimiento.

## Mecanismo de transferencia de safecoin
Contrariamente al bitcoin, la red SAFE no usa una blockchain para gestiona la propiedad de las monedas. Eso hace que los gestores de transferencia de la red SAFE no encadenen el historial de propietarios por lo que solo se guarda información del actual y el anterior dueño. Eso hace pensar que los safecoin se comportan como un sistema de moneda digital (eCash).

Uno de los mayores problemas de cualquier moneda virtual es evitar el doble gasto. En la red SAFE, la transferencia de datos, safecoin incluido, es [atómica] (http://es.wikipedia.org/wiki/Instrucci%C3%B3n_at%C3%B3mica) usando firmas digitales para trasferir la propiedad. 

Los safecoin, la moneda de la red SAFE, se genera en respuesta al uso de la red. Cuando un dato se guarda, o una aplicación se crea y usa, la red genera safecoin cada uno de ellos con su propia ID. En tanto en cuanto las monedas son divisibles, a cada nueva denominación se le asigna una ID única nunca utilizada.

Dado que cada moneda se asigna a un usuario de la red, solo dicho usuario específico puede transferir la propiedad de esa moneda gracias a una firma digital. Para fines ilustrativos, cuando Alicia realiza el pago de una moneda a Bob, a través del programa cliente, está solicitando a la red dicho pago. Los Transaction managers verifican que Alicia es la poseedora actual de la moneda recuperando su clave pública y confirmando que ha sido correctamente firmada con la clave privada. Solo entonces los Transaction manager aceptan esta petición de transferencia. La firma digital prueba, sin ninguna duda, que Alicia es la poseedora de la moneda, posesión que será transferida a Bob el cual pasará a ser el único con poder para volver a transferir esta moneda a otra persona. Este proceso se muestra en la Fig 3.

![](safecoin transfer mech.png)



