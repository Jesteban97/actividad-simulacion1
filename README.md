# Actividad de seguimiento - Simulación 1

|Integrante|correo|usuario github|
|---|---|---|
|Juan Esteban Aristizabal Aristizabal|jesteban.aristizabal@udea.edu.co|Jesteban97|
|Sharid Samantha Madrid Ospina|sharid.madrid@udea.edu.co|sharid.madrid@udea.edu.co|

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).

**Importante**:
* Como la actividad es en las parejas del laboratorio, solo uno de los integrantes tiene que hacer el fork; y sobre repositorio bifurcado que se genera, la modificación se realiza en equipo.
* Como la entrega se debe hacer modificando el archivo READNE, se recomienda que consulte mas sobre el lenguaje **Markdown**. En el repo adjuntan dos cheatsheet ([cheat sheet 1](Markdown_Cheat_Sheet.pdf), [cheatsheet 2](markdown-cheatsheet.pdf)) para consulta rapida.
* Entre mas creativo mejor.

## Homework (Simulation)

This program, [`process-run.py`](process-run.py), allows you to see how process states change as programs run and either use the CPU (e.g., perform an add instruction) or do I/O (e.g., send a request to a disk and wait for it to complete). See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-intro/README.md) for details.

### Questions

1. Run `process-run.py` with the following flags: `-l 5:100,5:100`. What should the CPU utilization be (e.g., the percent of time the CPU is in use?) Why do you know this? Use the `-c` and `-p` flags to see if you were right.
   *La solución se encuentra al desplegar abajo*
   <details>
   <summary>Answer</summary>
   Debería ser un *100%* puesto que al ejecutar el primer comando ambos procesos tanto el 0 como el 1 ejecutan en todos los momentos cpu. Al comprobar se puede evidenciar que efectivamente el consumo de la cpu es de 100%. al procesos de cpu oscilan solo entre los estados ready y running.
   - [x] Se aprovecha al máximo el recurso de la cpu.
   - [] No se aprovecha al máximo el recurso de la cpu
   ![Evidencia_1](/Evidencias/Evidencia1.png)
   </details>
   <br>

2. Now run with these flags: `./process-run.py -l 4:100,1:0`. These flags specify one process with 4 instructions (all to use the CPU), and one that simply issues an I/O and waits for it to be done. How long does it take to complete both processes? Use `-c` and `-p` to find out if you were right. 
   *La solución se encuentra al desplegar abajo*
   <details>
   <summary>Answer</summary>
   Debería tener una demora de *11 tiempos* debido a que el llamado a I/O genera un estado de blocked durante 5 momentos de tiempo, mientras que en los otros 6 se ejecuta los procesos en cpu los 4    usando el 100% de la cpu y el run y el done del proceso I/O. en este caso se oscila entre los 3 estados, ready, running y blocked para el proceso I/O y running y ready para los procesos CPU.
   - [] Se aprovecha al máximo el recurso de la cpu.
   - [x] No se aprovecha al máximo el recurso de la cpu
    ![Evidencia_2](/Evidencias/evidencia2.png)
   </details>
   <br>

3. Switch the order of the processes: `-l 1:0,4:100`. What happens now? Does switching the order matter? Why? (As always, use `-c` and `-p` to see if you were right)
   *La solución se encuentra al desplegar abajo*
   <details>
   <summary>Answer</summary>
   Para este caso al invertir deberia ocuparse la CPU por parte de los otros procesos mientras el I/O se ejecuta en el estado Blocked, *esto pasa por los 3 estados al cambiar de run a blocked el cpu queda libre para que se ejecuten los procesos que solo requieren CPU*,aquí el proceso deberia demorarse 7 momentos para procesar ambos programas y un mejor       
   aprovechamiento del cpu.
   - [x] Se aprovecha al máximo el recurso de la cpu.
   - [] No se aprovecha al máximo el recurso de la cpu
   ![Evidencia_3](/Evidencias/evidencia3.png)
   </details>
   <br>

4. We'll now explore some of the other flags. One important flag is `-S`, which determines how the system reacts when a process issues an I/O. With the flag set to SWITCH ON END, the system will NOT switch to another process while one is doing I/O, instead waiting until the process is completely finished. What happens when you run the following two processes (`-l 1:0,4:100 -c -S SWITCH ON END`), one doing I/O and the other doing CPU work?
   *La solución se encuentra al desplegar abajo*
   <details>
   <summary>Answer</summary>
   Efectivamente es como ocurre en el caso 2, se demora 11 momentos en realizar el procesamiento de ambos procesos, y tiene un % de CPU que no se utiliza cuando se ejecuta el proceso de I/O. Este comando hace que se tenga que ejecutar todo un proceso para empezar el siguiente, los procesos de CPU se quedan en espera(Ready) mientras la I/O finaliza.
   - [] Se aprovecha al máximo el recurso de la cpu.
   - [x] No se aprovecha al máximo el recurso de la cpu
   ![Evidencia_4](/Evidencias/evidencia4.png)
   </details>
   <br>

5. Now, run the same processes, but with the switching behavior set to switch to another process whenever one is WAITING for I/O (`-l 1:0,4:100 -c -S SWITCH ON IO`). What happens now? Use `-c` and `-p` to confirm that you are right.
   *La solución se encuentra al desplegar abajo*
   <details>
   <summary>Answer</summary>
   Con este comando hace que se ejecute de manera que cuando se entra al estado blocked por parte del I/O se aproveche el espacio para ejecutar los procesos en CPU, tal y como ocurre con el caso 3. Dando un mejor rendimiento ya que se aprovecha mejor el consumo de la CPU y disminuye los tiempos de ejecución.
   - [x] Se aprovecha al máximo el recurso de la cpu.
   - [] No se aprovecha al máximo el recurso de la cpu
   ![Evidencia_5](/Evidencias/evidencia5.png)
   </details>
   <br>

6. One other important behavior is what to do when an I/O completes. With `-I IO RUN LATER`, when an I/O completes, the process that issued it is not necessarily run right away; rather, whatever was running at the time keeps running. What happens when you run this combination of processes? (`./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER`) Are system resources being effectively utilized?
   *La solución se encuentra al desplegar abajo*
   <details>
   <summary>Answer</summary>
   En este caso sucede que todos los procesos que no son de I/O se ejecutan primera antes de que el primer proceso de I/O termine, es decir cuando pasa el primer proceso de I/O y este pasa de estado a blocked, comienza a ejecutar todos los procesos de CPU posponiendo la finalización de este proceso de I/0 y *esperando por 9 tiempos para ejecutar la finalización de este I/O*. Definitivamento no es el uso más efectivo de los recursos ya que en gran parte de las otras ejecuciones de I/O la cpu no se utiliza.
   - [] Se aprovecha al máximo el recurso de la cpu.
   - [x] No se aprovecha al máximo el recurso de la cpu
   ![Evidencia_6](/Evidencias/evidencia6.png)
   </details>
   <br>

7. Now run the same processes, but with `-I IO RUN IMMEDIATE` set, which immediately runs the process that issued the I/O. How does this behavior differ? Why might running a process that just completed an I/O again be a good idea?
   *La solución se encuentra al desplegar abajo*
   <details>
   <summary>Answer</summary>
   El comportamiento difiere en que ahora se *aprovechan todos los tiempos que los I/O se encuentran Blocked para ejecutar los otros procesos de CPU*. Es buena idea si se organizan de manera que siempre en esos tiempos donde el cpu de la I/O no se este utilizando pueda aprovecharse para las otras tareas de CPU.
   este la muestra del caso que deberia ser para poder optimizar el uso de los recursos.
   - [x] Se aprovecha al máximo el recurso de la cpu.
   - [] No se aprovecha al máximo el recurso de la cpu
   ![Evidencia_7](/Evidencias/Evidencia7.png)
   </details>
   <br>

