## Sistema de Controle

O **Sistema de controle** é a **Placa**, que é a raiz de todos os projetos. Da **Placa**, qualquer número de **[Componentes](#componentes)** podem ser conectadas e controladas. Exemplo: Arduino UNO, Electric Imp, BeagleBone Black, etc.

## Componente

Um **Componente** é qualquer sensor ou efetor. Um **Componente** é representado no programa por uma instância ou classe inicializada por um controlador apropriado **[Controlador](#controlador)**. Um **Componente** também pode ter uma sub-classificação na forma de um **[Dispositivo](#dispositivo)**. Exemplo: LED, LCD, Servo, sensor de temperatura.

## Controlador

Um **Controlador** contém comandos de programação gerais ou específicos de **Componentes** que estão em conformidade com uma interface comum.

## Dispositivo

Um **Dispositivo** é uma sub-classificação de um **Componente**. Motores são um bom exemplo: um motor pode ser "direcional" ou "não direcional".