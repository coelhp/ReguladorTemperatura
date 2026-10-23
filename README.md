# 🌡️ Controlador de Temperatura com Arduino Mega 2560

Projeto de controle de temperatura de um reservatório utilizando **Arduino Mega 2560**, sensor digital **DS18B20** e sistemas de aquecimento e refrigeração.

O sistema permite definir uma temperatura de referência, monitorar a temperatura medida e acionar os dispositivos responsáveis pelo aquecimento ou resfriamento, buscando manter o processo dentro de uma faixa de operação estabelecida.

O projeto integra conceitos de **instrumentação, eletrônica, programação embarcada e controle de processos**.

---

## 📌 Sumário

* [Objetivos](#-objetivos)
* [Visão geral do sistema](#-visão-geral-do-sistema)
* [Hardware utilizado](#-hardware-utilizado)

  * [Arduino Mega 2560](#1-arduino-mega-2560)
  * [Sensor de temperatura DS18B20](#2-sensor-de-temperatura-ds18b20)
  * [Display LCD](#3-display-lcd)
  * [Sistemas de aquecimento e refrigeração](#4-sistemas-de-aquecimento-e-refrigeração)
  * [Componentes auxiliares](#5-componentes-auxiliares)
* [Funcionamento do controle](#-funcionamento-do-controle)
* [Tecnologias e bibliotecas](#-tecnologias-e-bibliotecas)
* [Estrutura do repositório](#-estrutura-do-repositório)
* [Instalação e execução](#-instalação-e-execução)
* [Possíveis melhorias](#-possíveis-melhorias)
* [Referências técnicas](#-referências-técnicas)

---

## 🎯 Objetivos

* Implementar um sistema embarcado para monitoramento de temperatura.
* Utilizar um sensor digital para obter a temperatura do reservatório.
* Permitir a definição de uma temperatura de referência.
* Acionar sistemas de aquecimento e refrigeração de acordo com a temperatura medida.
* Apresentar informações de operação por meio de um display LCD.
* Aplicar conceitos de instrumentação e controle em um sistema físico.

## ⚙️ Visão geral do sistema

O controlador compara a temperatura medida pelo sensor com a temperatura de referência definida pelo usuário.

De acordo com essa comparação, o programa determina qual condição de operação deve ser aplicada:

| Condição                                  | Ação prevista                         |
| ----------------------------------------- | ------------------------------------- |
| Temperatura abaixo da faixa de referência | Acionamento do aquecimento            |
| Temperatura dentro da faixa de referência | Aquecimento e refrigeração desligados |
| Temperatura acima da faixa de referência  | Acionamento da refrigeração           |

O programa utiliza uma faixa de tolerância em torno da referência para evitar que o sistema dependa exclusivamente da igualdade entre a temperatura medida e a temperatura desejada.

> **Observação:** a lógica descrita corresponde à implementação presente no código-fonte. O desempenho real depende da montagem elétrica, dos dispositivos de potência e das características térmicas do reservatório.

---

## 🔌 Hardware utilizado

Esta seção apresenta os principais dispositivos utilizados no projeto, descrevendo suas características e sua função no sistema de controle.

### 1. Arduino Mega 2560

<p align="center">
  <img src="https://store.arduino.cc/cdn/shop/files/A000067_01.iso_ac4058f0-048a-497d-a924-c5e64ad45dfa.jpg?v=1727098253" alt="Placa Arduino Mega 2560 Rev3" width="500">
</p>

<p align="center">
  <em>Figura 1 — Placa de desenvolvimento Arduino Mega 2560 Rev3.</em><br>
  Fonte da imagem: <a href="https://store.arduino.cc/products/arduino-mega-2560-rev3">Arduino Official Store</a>.
</p>

O **Arduino Mega 2560** é a unidade responsável pela execução do algoritmo de controle. Ele recebe as informações do sensor, processa a temperatura medida, compara esse valor com a referência e comanda as saídas destinadas aos sistemas de aquecimento e refrigeração.

A placa é baseada no microcontrolador ATmega2560 e possui recursos adequados para projetos com múltiplos sensores, atuadores e interfaces.

**Características técnicas principais:**

| Característica           | Especificação |
| ------------------------ | ------------- |
| Microcontrolador         | ATmega2560    |
| Tensão de operação       | 5 V           |
| Frequência de clock      | 16 MHz        |
| Memória Flash            | 256 KB        |
| Entradas/saídas digitais | 54            |
| Entradas analógicas      | 16            |
| Saídas com suporte a PWM | 15            |

**Aplicação no projeto:**

* Leitura do sensor de temperatura.
* Processamento da lógica de controle.
* Leitura do comando de configuração da temperatura desejada.
* Acionamento dos relés e LEDs indicadores.
* Comunicação serial para apresentação de informações de diagnóstico.

Documentação técnica: [Arduino Mega 2560 Rev3](https://docs.arduino.cc/hardware/mega-2560).

### 2. Sensor de temperatura DS18B20

<p align="center">
  <img src="https://cdn.shopify.com/s/files/1/0174/1800/products/DS18B20.jpg?v=1597048328" alt="Sensor digital de temperatura DS18B20 com encapsulamento metálico e cabo" width="400">
</p>

<p align="center">
  <em>Figura 2 — Exemplo de sensor digital DS18B20 com sonda metálica.</em><br>
  Fonte da imagem: <a href="https://shop.pimoroni.com/products/ds18b20-programmable-resolution-1-wire-digital-thermometer">Pimoroni</a>.
</p>

O **DS18B20** é um sensor digital de temperatura que utiliza o protocolo de comunicação **1-Wire**. Ele permite realizar medições de temperatura por meio de uma interface digital, simplificando a comunicação com microcontroladores.

O sensor é responsável por fornecer a variável de processo utilizada pelo algoritmo de controle: a temperatura medida do sistema.

**Características técnicas do DS18B20:**

| Característica               | Especificação                 |
| ---------------------------- | ----------------------------- |
| Tipo de sensor               | Digital                       |
| Protocolo de comunicação     | 1-Wire                        |
| Faixa de medição             | −55 °C a +125 °C              |
| Resolução configurável       | 9 a 12 bits                   |
| Precisão típica especificada | ±0,5 °C entre −10 °C e +85 °C |

**Aplicação no projeto:**

O código utiliza as bibliotecas `OneWire` e `DallasTemperature` para estabelecer a comunicação com o sensor e obter a temperatura em graus Celsius.

Na implementação atual, o pino digital **13** do Arduino Mega 2560 é utilizado como conexão de dados do sensor.

A sonda metálica apresentada na imagem é uma versão comercial do DS18B20. O encapsulamento efetivamente utilizado na montagem deve ser confirmado com base nos componentes físicos do projeto.

Documentação técnica: [DS18B20 — Analog Devices](https://www.analog.com/en/products/DS18B20.html).

### 3. Display LCD

O display LCD é utilizado para apresentar informações ao operador durante a execução do programa.

O código-fonte configura um display de **16 colunas por 2 linhas**, utilizando a biblioteca `LiquidCrystal`.

Entre as informações apresentadas estão a temperatura medida, a temperatura de referência e indicações relacionadas ao estado do controle.

### 4. Sistemas de aquecimento e refrigeração

Os sistemas de aquecimento e refrigeração são os elementos responsáveis por alterar a temperatura do reservatório.

O código prevê duas saídas de controle:

| Elemento             | Pino definido no código | Função                             |
| -------------------- | ----------------------: | ---------------------------------- |
| Relé de refrigeração |                       6 | Comandar o sistema de refrigeração |
| Relé de aquecimento  |                       7 | Comandar o sistema de aquecimento  |

Os relés funcionam como elementos de interface entre as saídas de controle e os circuitos dos atuadores, conforme a montagem adotada.

**Atenção:** o circuito de potência deve ser dimensionado de acordo com as características elétricas dos dispositivos utilizados. Não se deve alimentar diretamente cargas de potência pelos pinos do Arduino.

### 5. Componentes auxiliares

Além dos componentes principais, o projeto utiliza elementos auxiliares para interação e indicação do estado de operação.

| Componente                           | Pino definido no código | Função                                  |
| ------------------------------------ | ----------------------: | --------------------------------------- |
| Botão de comando                     |                       1 | Alterar a etapa de operação do programa |
| LED indicador de aquecimento         |                       8 | Indicar o estado de aquecimento         |
| LED indicador de temperatura estável |                       9 | Indicar a condição de estabilidade      |
| LED indicador de refrigeração        |                      10 | Indicar o estado de refrigeração        |

Os pinos indicados correspondem às definições encontradas no código-fonte. Antes da montagem, recomenda-se verificar o circuito completo e confirmar a compatibilidade entre as conexões.

---

## 🔄 Funcionamento do controle

O funcionamento pode ser dividido nas seguintes etapas:

1. **Inicialização:** o Arduino configura o display, as entradas e saídas, a comunicação serial e as bibliotecas do sensor.
2. **Definição da referência:** o programa lê o sinal utilizado para definir a temperatura desejada.
3. **Leitura da temperatura:** o sensor DS18B20 fornece a temperatura atual do sistema.
4. **Comparação:** o programa compara a temperatura medida com os limites inferior e superior da faixa de referência.
5. **Acionamento:** o programa comanda o aquecimento ou a refrigeração, conforme a condição identificada.
6. **Indicação:** o display LCD, os LEDs e a comunicação serial fornecem informações sobre a operação.

### Lógica de controle

Seja:

* \(T_m\): temperatura medida pelo sensor;
* \(T_r\): temperatura de referência;
* \(T_{inf}\): limite inferior da faixa de controle;
* \(T_{sup}\): limite superior da faixa de controle.

Na implementação atual, os limites são calculados a partir da referência:

$T_{inf} = T_r - 2$

$T_{sup} = T_r + 2$

A lógica prevista pode ser resumida da seguinte forma:

$\text{Ação} =\begin{cases}\text{Aquecer}, & T_m < T_{inf} \\\text{Manter}, & T_{inf} < T_m < T_{sup} \\\text{Resfriar}, & T_m > T_{sup}\end{cases}$

O código também contém uma condição específica para quando a temperatura medida é igual à referência.

Essa estratégia se aproxima de um controle liga/desliga com faixa de tolerância. Ela não corresponde a um controlador PID: o acionamento dos atuadores é determinado pelas condições lógicas implementadas no programa.

---

## 💻 Tecnologias e bibliotecas

O projeto utiliza a linguagem de programação do ecossistema Arduino e bibliotecas específicas para comunicação com o sensor e com o display.

| Tecnologia ou biblioteca | Finalidade                                          |
| ------------------------ | --------------------------------------------------- |
| Arduino IDE              | Edição, compilação e envio do programa para a placa |
| `OneWire`                | Comunicação pelo protocolo 1-Wire                   |
| `DallasTemperature`      | Leitura e tratamento dos dados do DS18B20           |
| `LiquidCrystal`          | Controle do display LCD                             |
| Comunicação serial       | Apresentação de informações para diagnóstico        |

As bibliotecas `OneWire` e `DallasTemperature` devem estar instaladas no ambiente de desenvolvimento para que o programa seja compilado corretamente.

---

## 📁 Estrutura do repositório

```text
ReguladorTemperatura/
├── C_digo_Fonte_Arduino.ino
└── README.md
```

* **`C_digo_Fonte_Arduino.ino`**: código-fonte do controlador de temperatura.
* **`README.md`**: documentação do projeto, componentes e funcionamento.

Para organizar as imagens e facilitar a manutenção da documentação, recomenda-se ampliar a estrutura do repositório:

```text
ReguladorTemperatura/
├── C_digo_Fonte_Arduino.ino
├── README.md
└── assets/
    ├── arduino-mega-2560.jpg
    ├── sensor-ds18b20.jpg
    ├── display-lcd.jpg
    ├── sistema-aquecimento.jpg
    ├── sistema-refrigeracao.jpg
    └── montagem-projeto.jpg
```
---

## 🚀 Instalação e execução

### Pré-requisitos

* Arduino Mega 2560.
* Sensor DS18B20.
* Display LCD compatível com a biblioteca `LiquidCrystal`.
* Circuitos de acionamento para aquecimento e refrigeração.
* Componentes auxiliares necessários à montagem.
* Arduino IDE.
* Bibliotecas `OneWire` e `DallasTemperature`.

### Procedimento

1. Clone ou baixe este repositório.
2. Abra o arquivo `C_digo_Fonte_Arduino.ino` na Arduino IDE.
3. Instale as bibliotecas necessárias.
4. Conecte o Arduino Mega 2560 ao computador por meio do cabo USB.
5. Selecione a placa Arduino Mega 2560 e a porta serial correta.
6. Verifique as conexões do sensor, display, botão, LEDs e circuitos de acionamento.
7. Compile o programa e envie-o para a placa.
8. Execute os testes iniciais com os circuitos de potência em condição segura.

> **Importante:** o repositório contém o código-fonte, mas não apresenta, em sua estrutura atual, um diagrama elétrico completo da montagem. Antes de energizar o sistema, confirme as ligações, a alimentação e os requisitos de segurança de todos os componentes.

---

## 🔧 Possíveis melhorias

* Adicionar um diagrama elétrico completo da montagem.
* Incluir fotografias reais do protótipo em funcionamento.
* Documentar as conexões de todos os componentes.
* Explicar o procedimento de configuração da temperatura de referência.
* Melhorar o tratamento de falhas de comunicação com o sensor.
* Implementar uma estratégia de controle com histerese explicitamente definida.
* Registrar as medições em arquivo para análise do comportamento térmico.
* Criar gráficos da temperatura medida e da referência ao longo do tempo.
* Documentar os testes realizados e os resultados obtidos.

---

## 📚 Referências técnicas

* [Arduino Mega 2560 Rev3 — documentação oficial](https://docs.arduino.cc/hardware/mega-2560)
* [DS18B20 — documentação técnica do fabricante](https://www.analog.com/en/products/DS18B20.html)
* [Biblioteca OneWire — repositório do projeto](https://github.com/PaulStoffregen/OneWire)
* [Biblioteca DallasTemperature — repositório do projeto](https://github.com/milesburton/Arduino-Temperature-Control-Library)

---

## 🎓 Contexto acadêmico

Projeto voltado à aplicação prática de conceitos de instrumentação, sistemas embarcados e controle de temperatura, integrando sensores, processamento de sinais e acionamento de atuadores.

O repositório documenta a implementação do controlador e serve como referência para estudos e futuras melhorias em sistemas de controle térmico.
