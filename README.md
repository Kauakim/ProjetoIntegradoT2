# ProjetoIntegradoT2

Made by: Victor Alberti, Kauâ Ribeiro, Júlio Magalhães and Samuel Farias.

This project was developed in Brazil, so Readme.md is in portuguese.

O projeto visa criar um sistema de rádio automátizado para aviação, no qual a frequência de operação do dispositivo é alterada automaticamente baseado na área de voo que o piloto tenha selecionado, pois desse modo, a frequência do rádio não precisa ser configurada manualmente pela tripulação. A solução foi desenvolvida através de um sistema IoT, de forma a utilizar o protocolo MQTT, que transporta a informação de qual é a respectivo frequência a ser utilizada para comunicação naquela região. Eis como utilizar o sistema:

# Dependencias

1. O dispositvo que for utilizado para realizar a consulta ao Broker MQTT e a leitura Serial deve ser capaz de executar códigos na linguagem Python 3.11.2 e de conectar na internet.
2. É necessário possuir instalado no dispositvo de consulta as bibliotecas Python: PREENCHER
3. Moficaque o código "App.py" para que seja possivel ler a interface Serial:

  # Modificando o código da consulta ao broker (app.py):
  
  Afim de que o código funcione corretamente deve-se modificar a seção do código resposável pelo gerenciamento da Serial, no qual é necessário indicar a interface que será utilizado, isso pode ser feito na linha 17:
  
  ser = serial.Serial("{Sua interface, Ex: ttyUSB0}", baudrate=115200, timeout=1)
  
        Substitua {Sua interface, Ex: ttyUSB0} pelo nome da sua interface Serial.

# Sequência principal:

1. Execute o código de definição de frequência (controle.py). Durante a execução, informe qual frequência deve ser utilizada para cada área de voo.
Caso tudo ocorra tudo certo a mensagem "" aparecerá no console. Em caso de erro, observe a seção de erros no final do documento.

2. Execute o código "App.py" no dispositivo que fará a leitura Serial.
Caso tudo ocorra certo deve ser possivel observar no console as frequências obtidas através da comunicação do broker. Em caso de erro, observe a seção de erros no final do documento.

Feito os dois passos principais da utilização do sistema, pode-se seguir para a utilização painel de controle físico.

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

Sendo assim, inialmente o piloto deve escolher a área que o voo ocorrerá (Ex: Área C) e deve receber em resposta 3 flashes no led 1. Caso seja necessário trocar de área de voo durante o trajeto, basta o piloto escolher a nova respectiva área (Ex: Área E) que o sistema trocará o canal de comunicação automaticamente e novamente, o led 1 piscará 3 vezes para informar que ocorreu tudo certo.

Se em algum momento o led 2 começar a piscar, quer dizer que houve um problema de comunicação. Então, cabe ao piloto selecionar o modo de emergência manualmente. Ao realizar essa ação, o led 1 erá piscar 3 vezes para informar que houve a mudança modo.

# Modo de emergência

Considerações Imporantes:

1. O modo de emergência ativado pelo botão 6, troca a frequência de operação do rádio de aeronave para a de cárater emergêncial. Essa frequência é definida como aquela que possui o maior alcance, ou seja, a menor frequência de operação do rádio.
2. Qualquer frequência de qualquer outra área será ignorada enquanto o modo de emergência estiver ativo. Entretanto, quando desativado, a frequência recebida no periodo de atuação do modo emergencial será gravada automaticamente.
3. Para desetivar esse modo, basta precionar novamente o botão 6.

# Possiveis Erros:

Essa seção apresenta algumas soluções para possiveis erros que podem acontecer durante o processo de instalação do sistema.

Casos:




.Minhas obs:
tutorial de como mexer nos botões e entender os led's
criar seção de erros.
