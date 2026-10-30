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

<!-- TODO: Explicación de RAPID y funciones -->

## Resultado

### Decoración

<!-- TODO: Agregar imagen de tablero -->

### [Video](https://youtu.be/dHdbAhTnvL0)

```
https://youtu.be/dHdbAhTnvL0
```