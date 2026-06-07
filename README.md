Descripción general
El contrato implementa un sistema de apuestas descentralizado:

Los usuarios pueden apostar enviando ETH y eligiendo una opción.

Un admin controla el cierre de las apuestas.

Un oráculo carga el resultado del evento.

El contrato calcula los pagos proporcionales entre los ganadores y guarda un fondo de continuidad.

Se puede consultar un reporte final con todos los datos del evento.


1. Variables principales
* totalApuestas: suma de todo el dinero apostado.

* fondoContinuidad: porcentaje reservado (5%) para mantener el sistema.

* bolsaPremios: dinero destinado a repartir entre ganadores.

* eventoCerrado: indica si ya no se aceptan apuestas.

* admin: dirección que despliega el contrato y controla el cierre.

* oraculo: dirección autorizada para cargar el resultado.

* resultadoEvento: opción ganadora.

2. Registro de apostadores
Cada apostador se guarda en un struct con:

monto: cuánto apostó.

opcion: la opción elegida.

registrado: evita que apueste más de una vez.

apuestas es un mapping que relaciona direcciones con sus datos.

listaApostadores guarda todas las direcciones para poder recorrerlas.

3. Eventos
Se emiten en la blockchain para auditar:

Quién apostó y cuánto.

Resultado cargado por el oráculo.

Pagos realizados a ganadores.

Fondo de continuidad reservado.

4. Constructor
Define al admin como quien despliega el contrato.

Define al oráculo como la dirección pasada al constructor.

5. Modificadores
onlyAdmin: solo el admin puede cerrar apuestas.

onlyOraculo: solo el oráculo puede cargar el resultado.

6. Funciones principales
Apostar
Permite apostar enviando ETH (msg.value).

Verifica que el evento no esté cerrado y que el jugador no haya apostado antes.

Registra al jugador y actualiza totalApuestas.

Emite evento ApuestaRealizada.

Cerrar apuestas
El admin marca el evento como cerrado (eventoCerrado = true).

Cargar resultado
El oráculo carga el resultado.

Emite evento ResultadoCargado.

Llama a calcularPagos().

7. Calcular pagos
Calcula:

fondoContinuidad = 5% de totalApuestas.

bolsaPremios = resto del dinero.

Suma todas las apuestas de los ganadores.

Distribuye la bolsa proporcionalmente:

Cada ganador recibe (monto apostado * bolsaPremios) / sumaGanadores.

Usa call{value: pago} para transferir ETH.

Emite PagoRealizado.

8. Reporte final
Devuelve:

resultado: opción ganadora.

todosApostadores: lista completa de jugadores.

ganadores: lista de direcciones ganadoras.

pagos: cuánto le corresponde a cada ganador.

fondo: fondo de continuidad.

En resumen
Este contrato:

Recibe apuestas en ETH.

Admin controla el cierre.

Oráculo carga el resultado.

Calcula pagos proporcionales y reserva un fondo.

Permite auditar todo con un reporte final y eventos en la blockchain.



