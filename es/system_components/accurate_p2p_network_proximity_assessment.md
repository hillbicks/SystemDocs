# Evaluación precisa de la proximidad de red P2P

Para asegurarnos transferencias de datos entre Vaults rápidas y precisas, es vital, para la red SAFE, identificar otros Vaults que se encuentren próximos. La red necesita monitorizar el estatus de los Vaults y el estado de los datos continuamente. Para ello, todas las decisiones de la red se realizan solamente con el consenso de otros Vaults.

Para hacerlo, los Vaults usan una [tabla de Hash distribuida] (http://es.wikipedia.org/wiki/Tabla_de_hash_distribuida) (DHT) mejorada que gestiona la identificación y confirmación de los diferentes Vaults dentro de la misma área. Al estar optimizada para su eficiencia y precisión, permite una rapidísima reconfiguración global (entre 20ms y 5 seg.).
