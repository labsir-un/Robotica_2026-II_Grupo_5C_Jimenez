# Informe de Laboratorio 01

- [Informe de Laboratorio 01](#informe-de-laboratorio-01)
  - [Descripción de problema y solución](#descripción-de-problema-y-solución)
  - [Diseño de herramienta](#diseño-de-herramienta)
  - [Calibración](#calibración)
    - [Virtual](#virtual)
    - [Física](#física)
  - [Plano de planta](#plano-de-planta)
  - [Diagramas de flujo](#diagramas-de-flujo)
    - [Pose de mantenimiento](#pose-de-mantenimiento)
    - [Decoración de pastel](#decoración-de-pastel)
    - [Flujo completo](#flujo-completo)
  - [RAPID](#rapid)
  - [Resultado](#resultado)
    - [Decoración](#decoración)
    - [Video](#video)


## Descripción de problema y solución

Tomando inspiración del procedimiento de decoración detallada de pasteles, surge la posibilidad de dar uso a los robots ABB IRB140 y las bandas transportadoras del LabSIR de la Universidad de Colombia para desarrollar rutinas que cumplan con los requisitos para completar satisfactoriamente con el procedimiento de "decorado de un pastel" pero haciendo uso de un marcador y un tablero.

Con lo anterior se inicia el desarrollo de la solución con un [modelado CAD](#diseño-de-herramienta) de la herramienta necesaria para acomodar el marcador con el robot, el cual fue impreso en 3D. Con dicho modelado se pudo iniciar con las pruebas del robot y de [calibración](#calibración), tanto de manera virtual, con el software RobotStudio, como con con el robot físico real.

A partir de éste punto se decidió el "pastel" que se usaría para el desarrollo del resto del laboratorio, junto con una [toma de medidas](#plano-de-planta) para tomar las distancias del robot ABB IRB140 Caín, a su respectiva banda transportadora. Con todo esto se puede proceder a hacer uso del RobotStudio para el desarrollo de las rutinas.

## Diseño de herramienta

<!-- TODO: Pantallazos de Inventor -->

## Calibración

### Virtual

<!-- TODO: Agregar imagen de TCP en RobotStudio -->

### Física

```
Puntos - TCP

MODULE TCP_tool5C1

  LOCAL PERS robtarget pTCP_Point1:=[[510.4041,-124.3853,329.0527],[0.177410870790482,0.179303169250488,0.945089638233185,-0.207800999283791],[-1,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	LOCAL PERS robtarget pTCP_Point2:=[[440.4074,-124.2009,315.3134],[0.234813287854195,0.96746951341629,0.0692992135882378,0.0637418776750565],[-1,-1,2,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	LOCAL PERS robtarget pTCP_Point3:=[[434.3988,-205.6365,325.5618],[0.268260836601257,0.559318363666534,0.77782267332077,0.100950442254543],[-1,-2,1,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	LOCAL PERS robtarget pTCP_Point4:=[[447.5572,-87.40168,297.1351],[0.0622128583490849,-0.584803104400635,0.753553032875061,-0.293756306171417],[-1,1,1,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];

ENDMODULE

MODULE Module5C
	TASK PERS tooldata tool5C:=[TRUE,[[45.9096,-3.00601,168.305],[1,0,0,0]],[1,[0,0,0],[1,0,0,0],0,0,0]];

ENDMODULE
```

## Plano de planta

<!-- TODO: Descripción de medidas para planta en RobotStudio -->

## Diagramas de flujo

### Pose de mantenimiento

[Diagrama de flujo de pose de mantenimiento](./flowCharts/flujoMantenimiento.md)

### Decoración de pastel

[Diagrama de flujo de decoración de pastel](./flowCharts/flujoDecoracion.md)

### Flujo completo

[Diagrama de flujo de rutina completa](./flowCharts/flujoResumen.md)

## RAPID

En esta sección se analiza y justifica la estructura, sintaxis y lógica del programa escrito en lenguaje **RAPID** para la celda de automatización robótica de decoración de pasteles con un robot ABB. Se detallan las funciones utilizadas, la declaración de datos de espacio y herramientas, y las decisiones de diseño implementadas para garantizar la seguridad, precisión y estabilidad del proceso.

---

## 1. Declaración de Datos y Configuración Espacial

El módulo `Module1` inicia definiendo los datos cinemáticos y de posición necesarios para que el controlador ejecute las trayectorias de forma precisa:

* **`PERS tooldata toolMarcador`**: Declara los datos de la herramienta del robot (marcador) con persistencia (`PERS`).
  * **Explicación y Justificación:** Especifica el *Tool Center Point* (TCP) definiendo la distancia desde la brida del robot al extremo útil del marcador (`[47.097, 0, 161.575]` mm) y su orientación mediante cuaternios. La persistencia `PERS` permite que el estado de la herramienta se mantenga actualizado entre ejecuciones del programa. Definir el TCP adecuado asegura que todas las rotaciones y trazados se realicen pivoteando exactamente sobre la punta del marcador.
* **`TASK PERS wobjdata Workobject_1`**: Declara el sistema de coordenadas del objeto de trabajo con persistencia de tarea (`TASK PERS`).
  * **Explicación y Justificación:** Asigna una matriz de transformación cartesiana para la superficie sobre la cual se dibuja. El uso de un *Work Object* independiza las trayectorias del robot de la base del manipulador. Si la mesa o banda transportadora se reubica físicamente, únicamente se recalibra la matriz de `Workobject_1`, conservando la validez de los más de 100 puntos (`robtarget`) de letras y figuras preprogramados.
* **`CONST robtarget Target_home` y `Target_maintenance`**: Puntos de referencia definidos como constantes (`CONST`).
  * **Explicación y Justificación:** Definen las coordenadas cartesianas ($X, Y, Z$), orientación en cuaternios, configuración de articulaciones y ejes externos (`9E+09` indica eje no utilizado) para la posición de reposo (`HOME`) y la postura de servicio (`Target_maintenance`). El modificador `CONST` protege estas posiciones críticas contra modificaciones accidentales durante la ejecución del programa.
* **`CONST robtarget Target_T_*`, `Target_i_*`, `Target_helmet_*`, etc.**: Coordenadas objetivo de trazado.
  * **Explicación y Justificación:** Definen la geometría tridimensional de las letras del nombre y detalles de los cascos sobre el pastel.

---

## 2. Estructura y Lógica del Bucle Principal (`main`)

El procedimiento principal `main()` gestiona el ciclo de vida del programa y la respuesta a eventos de la celda:

```rapid
PROC main()
    WHILE TRUE DO
        Reset DO_01;
        Reset DO_02;
        Reset DO_03;
        Reset FWD_Conveyor;
        Reset BWD_Conveyor;
        
        IF DI_01 = 1 THEN ... ENDIF
        IF DI_02 = 1 THEN ... ENDIF
        IF DI_03 = 1 THEN ... ENDIF
    ENDWHILE
ENDPROC
```

* **Estructura de Bucle Continuo (`WHILE TRUE DO`)**:
  * **Decisión de Diseño:** Implementa un bucle infinito que mantiene al robot en monitoreo permanente. Esto evita la finalización del programa y garantiza que la celda industrial responda de forma continua a los comandos del operador o de la línea de producción.
* **Restablecimiento Preventivo de Salidas (`Reset`)**:
  * **Decisión de Diseño:** Al inicio de cada iteración se fuerzan a `0` las salidas digitales de señalización (`DO_01`, `DO_02`, `DO_03`) y de activación de banda (`FWD_Conveyor`, `BWD_Conveyor`). Esto asegura un estado inicial seguro antes de evaluar cualquier nueva instrucción, previniendo activaciones no deseadas de los motores o falsas indicaciones de estado.

---

## 3. Análisis Detallado de Rutinas y Toma de Decisiones

La lógica del programa utiliza estructuras condicionales `IF ... THEN` para seleccionar la rutina a ejecutar según las entradas digitales (`DI_01`, `DI_02`, `DI_03`):

### 3.1. Rutina de Mantenimiento (`DI_01 = 1`)

```rapid
IF DI_01 = 1 THEN
    Set DO_01;
    Path_home;
    Path_maintenance;
    WHILE DI_01 = 0 DO
        WaitTime(5);
    ENDWHILE
    Path_home;
    Reset DO_01;
ENDIF
```

* **Funciones e Instrucciones Utilizadas:** `Set`, `Reset`, `MoveJ`, `WHILE ... DO`, `WaitTime`.
* **Decisiones de Diseño y Justificación:**
  1. **Señalización Explicita:** Se activa `Set DO_01` para indicar a la celda o PLC externo que el robot entra en modo de servicio.
  2. **Trayectoria de Aproximación Segura (`Path_home` $\rightarrow$ `Path_maintenance`):** Antes de ir a la posición de mantenimiento, el robot transita por `Target_home`. Esto eleva el brazo y evita colisiones mecánicas con la banda transportadora o el pastel.
  3. **Retención Pasiva en Mantenimiento:** El bucle `WHILE DI_01 = 0 DO WaitTime(5); ENDWHILE` mantiene al manipulador en la pose de servicio realizando pausas periódicas de 5 segundos hasta que se modifique el estado de la entrada.
  4. **Retorno Seguro:** Al concluir la condición, el robot regresa a `Path_home` y apaga la señal `DO_01`, dejando la celda lista para producción.

### 3.2. Rutina Auxiliar / Farmeo de Aura (`DI_02 = 1`)

```rapid
IF DI_02 = 1 THEN
    Set DO_02;
    Path_10;
    Reset DO_02;
ENDIF
```

* **Funciones e Instrucciones Utilizadas:** `Set`, `Reset`, `MoveJ`.
* **Decisiones de Diseño y Justificación:**
  1. **Aislamiento de Rutina:** Delimita el inicio y fin de la tarea mediante la salida `DO_02`.
  2. **Verificación Articular (`Path_10`):** Ejecuta una secuencia de movimientos articulares pasando por `Target_home`, `Target_30`, `Target_40`, alternando repetidamente entre los puntos `Target_10`, `Target_20`, `Target_50`, `Target_60`, y finalizando en `Target_70`.
  3. **Justificación:** Permite realizar pruebas de dinámica, diagnóstico de ejes y verificación de repetibilidad en una zona libre de la celda sin activar la banda ni interferir con el área de trabajo del pastel.

### 3.3. Ciclo de Decoración de Pasteles (`DI_03 = 1`)

```rapid
IF DI_03 = 1 THEN
    Set DO_03;
    Move_banda_fwd;
    WaitTime 3;
    Path_T; Path_i; Path_tmin; Path_o; Path_dash;
    Path_J; Path_u; Path_a; Path_n;
    Path_helmet_1; Path_helmet_2; Path_helmet_3; Path_helmet_4; Path_helmet_5;
    Path_home;
    Move_banda_aux;
    Reset DO_03;
ENDIF
```

* **Funciones e Instrucciones Utilizadas:** `Set`, `Reset`, `WaitTime`, `MoveJ`, `MoveL`, llamadas a subrutinas (`PROC`).
* **Decisiones de Diseño y Justificación:**
  1. **Control de Avance de Banda (`Move_banda_fwd`):** Ejecuta la subrutina que activa los relés/entradas del motor de la banda transportadora (`Set FWD_Conveyor; Set BWD_Conveyor; WaitTime(3); Reset BWD_Conveyor;`) para desplazar el pastel entrante hacia la estación de trabajo.
  2. **Tiempo de Estabilización Mecánica (`WaitTime 3`):** Añade una pausa deliberada de 3 segundos con la banda totalmente detenida antes de iniciar los movimientos del robot. Esto elimina vibraciones y oscilaciones en la masa del pastel, garantizando que la superficie esté estática al iniciar el trazo.
  3. **Modularidad del Trazado (`Path_T` a `Path_helmet_5`):** Cada carácter y elemento gráfico está separado en su propio procedimiento (`PROC`). Esto facilita la depuración individual de letras, la modificación de geometrías y el mantenimiento del código.
  4. **Despeje de Herramienta (`Path_home`):** Una vez concluida la decoración, el robot se retira hacia la posición de reposo superior.
  5. **Evacuación del Producto (`Move_banda_aux`):** Activa un impulso de banda de 2.5 segundos para retirar el pastel decorado de la zona de trabajo.

---

## 4. Funciones e Instrucciones RAPID Utilizadas

A continuación se resume el catálogo de instrucciones y tipos de datos RAPID empleados en el programa, justificando su selección técnica:

| Instrucción / Tipo de Dato | Uso en el Código | Justificación Técnica |
| :--- | :--- | :--- |
| **`MoveJ`** *(Joint Motion)* | `MoveJ Target_home, v100, z1, toolMarcador\WObj:=wobj0;` | Realiza movimientos articulares coordinados no lineales. Se utiliza para aproximaciones rápidas en el aire, previniendo singularidades cinemáticas y minimizando el tiempo de ciclo. |
| **`MoveL`** *(Linear Motion)* | `MoveL Target_T_20, v100, z1, toolMarcador\WObj:=Workobject_1;` | Mantiene el TCP en una trayectoria estrictamente lineal a velocidad constante. Se aplica en todos los trazos sobre el pastel para asegurar líneas rectas y curvas homogéneas. |
| **`v100`** *(speeddata)* | Parámetro de velocidad en instrucciones `MoveJ` y `MoveL`. | Establece una velocidad TCP de $100\text{ mm/s}$, idónea para mantener la precisión del trazo del marcador sin dañar la superficie del pastel. |
| **`z1`** *(zonedata)* | Parámetro de zona de precisión en trayectorias. | Define un radio de redondeo de $1\text{ mm}$ en los puntos intermedios. Evita detenciones exactas bruscas (`fine`) entre vértices de letras, logrando un trazado continuo y fluido. |
| **`Set` / `Reset`** | `Set DO_01;`, `Reset FWD_Conveyor;` | Controlan salidas digitales discreta de la controladora IRC5 para activar o desactivar elementos de señalización y relés del motor de la banda. |
| **`WaitTime`** | `WaitTime 3;`, `WaitTime(5);` | Introduce retardo en segundos en la ejecución de la tarea robótica. Permite la sincronización temporal con la física del proceso (estabilización de la banda). |
| **`PROC ... ENDPROC`** | `PROC Path_T() ... ENDPROC` | Define subrutinas modularizadas para organizar el código en bloques reutilizables y legibles. |
| **`WHILE ... DO`** | `WHILE TRUE DO`, `WHILE DI_01 = 0 DO` | Controla estructuras iterativas infinitas o supeditadas a estados de señales digitales. |

## Resultado

### Decoración

<!-- TODO: Agregar imagen de tablero -->

### [Video](https://youtu.be/dHdbAhTnvL0)

```
https://youtu.be/dHdbAhTnvL0
```
