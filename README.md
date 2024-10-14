# Telhado Retratil que Reage Chuva
## Descrição: 

Este projeto consiste em um sistema de telhado retrátil automatizado, que utiliza um sensor de chuva para detectar mudanças no clima e acionar o fechamento do telhado automaticamente. O sistema é composto por um motor elétrico que controla o movimento do telhado e um sensor de chuva que, ao detectar água, envia um sinal para o motor, iniciando o fechamento do telhado de forma automática. Assim, o ambiente coberto é protegido de intempéries sem a necessidade de intervenção manual. O telhado abre novamente automaticamente quando o sensor não detecta mais chuva.

## Componentes:  

- 1 Arduino 
- 1 arduino Uno com cabo USB
- 1 sensor de chuva
- 1 protoboard
- 1 servomotor 9G
- 2 botões
- 3 LEDs
- 3 resistores de 330  Ω
- 2 resistores de 10 kΩ
- Sistema Operacional: Windows 10 / Ubuntu 20.04

  ## Codigo do Projeto


````````````
  #include <Servo.h>
Servo servo;

int cobertura = 11;          // Servo motor que controla a cobertura
int botaoauto = 8;          // Botao para o modo automatico
int botaomanual = 7;        // Botao para o modo manual 
int LEDauto = 6;            // LED liga se a programaçao esta automatica
int LEDaberto = 5;          // LED liga se a cobertura esta aberta
int LEDfechado = 4;         // LED liga se a cobertura esta fechada
int sensor = 3;             // Detecta da chuva
int leitura = A1;           // Medida dada pelo sensor de chuva
boolean estado = true;      // Variavel de estado da cobertura
boolean automatic = true;   // Variavel de estado do modo automatico

void setup( ) {
  pinMode(cobertura,   OUTPUT);
  pinMode(botaoauto,   INPUT);
  pinMode(botaomanual, INPUT);
  pinMode(LEDauto,     OUTPUT);
  pinMode(LEDaberto,   OUTPUT);
  pinMode(LEDfechado,  OUTPUT);
  pinMode(sensor,      INPUT);
  pinMode(leitura,     INPUT);

  servo.attach(cobertura);
  servo.write(90);           // cobertura aberta
  
  Serial.begin(9600);
  digitalWrite(LEDauto, HIGH);
}

void loop( ) {

  if(digitalRead(botaomanual) == HIGH){   // Botao ativa o modo manual
    estado = !estado;
    automatic = false;
    digitalWrite(LEDauto, LOW);
    
    if(estado ==true){                    // Cobertura manual abre
      digitalWrite(LEDaberto, HIGH);
      digitalWrite(LEDfechado, LOW);
      servo.write(150);
    }

    else{                                 // Cobertura manual fecha
      digitalWrite(LEDaberto, LOW);
      digitalWrite(LEDfechado, HIGH);
      servo.write(0);
    }
  }

  if(digitalRead(botaoauto) == HIGH){     // Botao ativa o modo automatico
    automatic = true;
    digitalWrite(LEDauto, HIGH);
  }
  
  if(digitalRead(sensor) == HIGH &  & automatic == true){  // Sem chuva detectada
    servo.write(150);
    digitalWrite(LEDaberto, HIGH);
    digitalWrite(LEDfechado, LOW);
  }
  
  if(digitalRead(sensor) == LOW && automatic == true){   // Chuva detectada
    servo.write(0);
    digitalWrite(LEDaberto, LOW);
    digitalWrite(LEDfechado, HIGH);
  }

  Serial.print("Medida do sensor: ");
  Serial.println(analogRead(leitura));  
  delay(250);
}

````````````

## Esquemas

![image](https://github.com/user-attachments/assets/d790d9aa-44cc-4c89-b113-23f0b0a04671)


## Bibliotecas Necessárias

Antes de começar, certifique-se de que as seguintes bibliotecas estão instaladas no seu Arduino IDE:

Servo.h (já incluída no Arduino IDE)
Esta biblioteca é necessária para controlar o servo motor que será responsável por abrir e fechar a cobertura. Não é necessário instalar, pois já está incluída no Arduino IDE.

## Passo a passo para montar o circuito

- 1 . Conectando o Sensor de Chuva:

O sensor de chuva tem 4 pinos: VCC, GND, A0 (analógico) e D0 (digital).
Conecte o VCC ao pino 5V do Arduino.
Conecte o GND ao pino GND do Arduino.
Conecte o D0 ao pino digital 3 do Arduino (detecção de chuva).
Conecte o A0 ao pino A1 (entrada analógica para medir a intensidade da chuva).

- 2 . Conectando o Servomotor 9G:

O servomotor tem 3 fios: Vermelho (VCC), Preto (GND) e Sinal (PWM).
Conecte o VCC do servo ao pino 5V do Arduino.
Conecte o GND do servo ao GND do Arduino.
Conecte o fio de Sinal ao pino digital 11 do Arduino.

- 3 . Conectando os Botões :
- 
Conecte os dois botões
Um botão no pino digital 8 (modo automático).
O outro no pino digital 7 (modo manual).
Coloque resistores de 10kΩ em cada botão para conectá-los ao GND.

- 4 . Conectando os LEDs:

Conecte os três LEDs:
LED 1 no pino digital 6 (indica modo automático).
LED 2 no pino digital 5 (indica cobertura aberta).
LED 3 no pino digital 4 (indica cobertura fechada).
Utilize resistores de 330Ω para cada LED, conectando-os ao GND.

- 5 . Upload do Código no Arduino
     
Abra o Arduino IDE no seu computador e conecte o Arduino Uno via cabo USB.
Escreva ou cole o código fornecido no exercício no Arduino IDE.
Verifique se tudo está correto clicando em Verificar (ícone de check) no IDE.
Após verificar, clique em Carregar (ícone de seta) para enviar o código para o Arduino.

- 6 . Teste do Sistema
 
Modo automático:
Pressione o botão de modo automático. O LED indicador de modo automático acenderá.
Se o sensor de chuva detectar água, o servomotor fechará a cobertura.
Se o sensor de chuva não detectar água, o servomotor abrirá a cobertura.
Modo manual:

Pressione o botão de modo manual para alternar entre abrir e fechar a cobertura manualmente.
O LED indicador de cobertura aberta ou fechada acenderá dependendo da posição da cobertura.

# Dicas Finais:

- Ajuste a sensibilidade do sensor de chuva se necessário, utilizando uma chave de fenda na placa do sensor.
- 
- Certifique-se de que a estrutura da cobertura esteja bem fixada para que o servo consiga movê-la suavemente.


