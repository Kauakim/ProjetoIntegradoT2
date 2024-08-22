# ProjetoIntegradoT2

Made by: Victor Alberti, Kauâ Ribeiro, Júlio Magalhães e Samuel Farias.

This project was developed in Brazil, so Readme.md is in portuguese.

O projeto visa criar um sistema de rádio automátizado para aviação, no qual a frequência de operação do dispositivo é alterada automaticamente baseado na área de voo que o piloto tenha selecionado, pois desse modo, a frequência do rádio não precisa ser configurada manualmente pela tripulação. A solução foi desenvolvida através de um sistema IoT, de forma a utilizar o protocolo MQTT, que transporta a informação de qual é a respectivo frequência a ser utilizada para comunicação naquela região. Eis como utilizar o sistema:


# Dependências

1. O dispositvo que for utilizado para realizar a consulta ao Broker MQTT e a comunicação Serial (Nesse caso uma Raspberry Pi) deve ser capaz de executar códigos na linguagem Python 3.11.2 e de se conectar na internet
2. É necessário instalar no dispositvo de consulta (Raspberry Pi) as bibliotecas Paho-MQTT, os, json, serial e time
3. Moficaque o código "App.py" para que seja possivel ler a interface Serial:

  # Modificando o código da consulta ao broker (app.py):
  
  Afim de que o código funcione corretamente deve-se modificar a seção do código resposável pelo gerenciamento da Serial, no qual é necessário indicar a interface que será utilizado, isso pode ser feito na linha 17:
  
  ser = serial.Serial("{Sua interface, Ex: ttyUSB0}", baudrate=115200, timeout=1)
  
        Substitua {Sua interface, Ex: ttyUSB0} pelo nome da sua interface Serial.


# Sequência principal:

1. Execute o código de definição de frequência (tower.py). Durante a execução, informe qual frequência deve ser utilizada para cada uma das áreas de voo.
Caso esse passo funcione, a mensagem "[INFO] Published!" aparecerá no terminal do dispositivo (Nesse caso, o computador da torre de comando), caso contrário, um erro, que pode ser observado na seção de erros no final do documento, ocorreu.

2. Execute o código "App.py" no dispositivo que fará a leitura Serial (Raspberry Pi).
Caso tudo ocorra certo, deve ser possivel observar no terminal do aparelho as frequências obtidas através da comunicação com o broker via MQTT, caso contrário, observe a seção de erros no final do documento para solucionar o problema apresentado.

Feito os dois passos principais descritos acima para a devida utilização do sistema, pode-se seguir para a utilização painel de controle físico.


# Utilização do painel de controle

O painel possui 6 botões e 2 led's, com os respetivos usos:
      
      botão 1 ---> Indica que a frequência da Área A deve ser utilizada.
      botão 2 ---> Indica que a frequência da Área B deve ser utilizada.
      botão 3 ---> Indica que a frequência da Área C deve ser utilizada.
      botão 4 ---> Indica que a frequência da Área D deve ser utilizada.
      botão 5 ---> Indica que a frequência da Área E deve ser utilizada.
      botão 6 ---> Ativa o modo de emergência, seu uso será detalhado na próxima seção.

      Led 1 ---> Indica que a frequência foi trocada e gravada ao piscar 3 vezes.
      Led 2 ---> Indica que houve uma perda de conexão com o broker e portanto não é possivel obter a frequência de comunicação.

Com base nessas informações, inicialmente o piloto deve escolher a área de voo em que se encontra no presente momento (Ex: Área C), o que por sua vez altera a frequência do rádio do avião e consequentemente faz com que 3 flashes no LED 1 surjam. Assim, caso seja a área de voo seja alterada durante o trajeto, o piloto deve, novamente, escolher a área em que se encontra (Ex: Área E), permitindo que o sistema troque o canal de comunicação automaticamente, e novamente, o LED 1 piscará 3 vezes, informando ao piloto que tudo ocorreu como o esperado.

Se em algum momento o LED 2 começar a piscar, um problema de comunicação está acontecendo no sistema, logo, cabe ao piloto selecionar o modo de emergência manualmente. Ao realizar essa ação, o LED 1 pisca 3 vezes novamente, informando que o canal de comunicação foi novamente alterado.


# Modo de emergência

Considerações Imporantes:

1. O modo de emergência, ativado pelo botão 6 do painel de controle do avião, troca a frequência de operação do rádio da aeronave para a de cárater emergêncial, cuja frequência é a que possui o maior alcance, ou seja, a menor frequência de operação do rádioS.
2. Qualquer frequência, de qualquer outra área, será ignorada enquanto o modo de emergência estiver ativo, garantindo que o piloto consiga se comunicar com a torre de comando. Entretanto, quando desativado, a frequência recebida no periodo de atuação do modo emergencial é gravada automaticamente, retornando o avião para seu modo normal de operação.
3. Para desetivar ou ativar esse modo, o piloto só precisa pressionar o botão 6.
4. Caso o LED 2 para de piscar, o sistema, que apresentou uma falha anteriormente, voltou a funcionar normalmente, logo, o modo de emergência deve ser manualmente desativado pelo piloto.
5. Caso o piloto pressione o botão que troca a área correspondente à qual o avião está localizado e o LED 1 ainda sim não pisque, o modo de emergência deve ser manualmente ativado pelo piloto. 


# Possiveis Erros:

Essa seção apresenta algumas das possiveis soluções para que os erros, que podem acontecer durante a utilização do sistema, sejam resolvidos.

1. Caso o dispositivo de consulta ao broker MQTT (Raspberry Pi) não consiga coletar ou publicar algo no Ubidots, convém ao usuário reiniciar o sistema e conferir se o dispositivo do projeto no Ubidots está funcionando como deveria, já que o plano gratuito ofertado pela plataforma sai do ar caso muitas requisições sejam feitas a ele em um curto espaço de tempo.
2. Caso a comunicação serial apresente falhas constantes, confira, no código do Arduino do painel de controle do avião, se a variável de debug está desativada. Desconecte também todos os dispositivos conectados nas outras portas USBs do dispositivo de consulta ao Broker (Raspberry Pi), já que esses aparelhos podem atrapalhar a comunicação. Por último, verifique se o dispositivo que se conecta a internet (Raspberry Pi) consegue realmente se comunicar com o Arduino do painel de controle do avião.
3. Caso o projeto esteja funcionando como deveria, porém o canal de comunicação ainda não está sendo alterado automaticamente, confira se o rádio utilizado no avião está funcionando e se a frequência inserida no broker MQTT para a área escolhida realmente pode ser utilizada no rádio.
