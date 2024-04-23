# IoT.SE.Opt.T - Virtual guitar pedal board (Pedaleira Virtual para Guitarras)

## Motivação

A Pedaleira Virtual para Guitarras surge da necessidade de proporcionar aos guitarristas uma solução prática e versátil para experimentar e criar efeitos sonoros sem a necessidade de equipamentos físicos. Com a Pedaleira Virtual, os músicos podem explorar uma ampla gama de efeitos de guitarra diretamente de seus computadores, facilitando o processo de composição, prática e gravação de músicas.

## Objetivos

- Oferecer uma experiência de pedaleira de guitarra completa e intuitiva através de uma aplicação virtual.
- Permitir aos usuários experimentar uma variedade de efeitos sonoros, incluindo distorção, delay, reverb, chorus, entre outros.
- Facilitar a criação e personalização de presets de efeitos para diferentes estilos musicais e preferências individuais.
- Integrar funcionalidades avançadas, como simulações de amplificadores e modelagem de gabinetes, para uma experiência sonora mais autêntica.
- Fornecer uma plataforma flexível e de código aberto para futuras expansões e colaborações da comunidade de músicos e desenvolvedores.

## Arquitetura

### Construção do projeto

Como arquitetura principal do projeto utiliza-se o seguinte esquema:


<img src="Images/MainCircuit.png" alt="Arquitetura" width="" height="">

Onde tem-se: 

- Based in Raspberry Pi Zero (1GHz ARM11 core).
- Analog stages using MCP6002 rail-to-rail operational amplifier.
- ADC: 12bits / Sampling Rate 50Ksps (MCP3202).
- Output Stage: 12 bits (2x6bits PWMs running in parallel)
- Pi Zero:
    - 1GHz ARM11 core.
    - 512MB of LPDDR2 SDRAM.
    - Micro-SD card slot.
- Connectors:
    - Input Jack, 1/4 inch unbalanced, Zin=0.5MΩ.
    - Output Jack, 1/4 inch unbalanced, Zout=100Ω.
    - Power supply: power taken from the Pi Zero board (micro-USB).

<img src="Images/Components.png" alt="Componentes" width="600" height="600">

### Funcionamento

- The Input Stage: Amplifies and filters the guitar signal making it ready for the ADC (Analog to Digital Converter). The ADC sends the signal to the PI ZERO using SPI communication.
  
- Pi ZERO: It takes the digitalized audio waveform from the ADC and does all the Digital Signal Processing (DSP) creating effects (distortion, fuzz, delay, echo, tremolo...) que, por sua vez, devem ser pré selecionados a partir da dashboard no aplicativo no dispositivo móvel do usuário. A partir da seleção do usuário o aplicativo estabelece comunicação com a placa a partir de comunicação MQTT.
  
- The Output Stage: Once the new digital waveform is created, the Pi Zero creates an analog signal with two PWMs combined, the signal is filtered and prepared to be sent to the next pedal or the guitar amp.
  
Portanto o funcionamento pode ser representado pelas seguintes imagens:

<img src="Images/AmpCircuit.png" alt="Funcionamento" width="" height="">

<img src="Images/ComunicationProcess.png" alt="Comunicação" width="1200" height="600">

### Aplicativo e dashboard

Para a criação do aplicativo pretende-se utilizar o software Godot, amplamente usado para o desenvolvimento de jogos, possibilitando assim um aplicativo intuitivo e funcional. Pretende-se criar um sistema de login para o usuário, para que seja possível salvar os estilos musicais e perfis de pedaleira, possibilitando uma maior personalização por parte do usuário.

Tem-se a seguinte imagem como conceito de tela de login:


<img src="Images/loginScreenConceptualImage.jpg" alt="Descrição da Imagem" width="600" height="400">

Como a seguinte imagem como inspiração para crição da dashboard:

<img src="Images/DashboardInspiration.png" alt="Dashboard" width="" height="">


## Manual de Instalação e Configuração

### Instalação e utilização do arduino cloud

#### Instalação do Arduino Create Agent:

- Acessa-se o site oficial do Arduino Cloud em https://create.arduino.cc/.
- Faz-se login na conta Arduino ou cria-se uma nova, se necessário.
- Na página inicial, clica-se em "Get Started".
- Sigue-se as instruções para baixar e instalar o Arduino Create Agent, que é necessário para conectar as placas Arduino ao Arduino Cloud.
- Depois de instalado, abre-se o Arduino Create Agent e faz-se login com as credenciais do Arduino.

#### Configuração do Arduino Cloud:

- Com o Arduino Create Agent aberto e logado, conecta-se a placa Arduino ao computador utilizando um cabo USB.
- Abre-se o navegador da web e acessa-se o Arduino Web Editor em https://create.arduino.cc/editor.
- No Arduino Web Editor, clica-se no ícone de configurações no canto superior direito da tela.
- Seleciona-se "Arduino Cloud" nas opções de configuração.
- Seleciona-se a placa Arduino que está sendo utilizada na lista de placas suportadas.
- Sigue-se as instruções para conectar sua placa Arduino ao Arduino Cloud através do Arduino Create Agent. Isso geralmente envolve emparelhar a placa com sua conta Arduino.

#### Utilização do Arduino Cloud:
- Com a placa Arduino conectada ao Arduino Cloud, pode-se começar a criar e carregar sketches diretamente do Arduino Web Editor.
- No Arduino Web Editor, clica-se em "Novo Sketch" para criar um novo código.
- Escreve-se o código do seu sketch na área de edição.
- Quando estiver pronto para carregar o sketch na placa Arduino, clica-se no botão "Upload" na barra de ferramentas.
- O código será compilado e carregado na placa Arduino automaticamente.
- Pode-se monitorar a saída serial da placa Arduino diretamente no Arduino Web Editor clicando em "Monitor Serial" na barra de ferramentas.

### Instalação e utilização do Godot Engine

#### Instalação do Godot

- Acessa-se o site oficial do Godot em https://godotengine.org/.
- Clica-se no botão "Download" ou "Baixar" na página inicial.
- Escolhe-se a versão do Godot que deseja baixar. Recomenda-se baixar a versão mais recente estável.
- Seleciona-se o sistema operacional correto para o qual deseja baixar o Godot (Windows, macOS, Linux).
- Faz-se o download do instalador adequado para o seu sistema operacional.
- Após o download, executa-se o instalador e sigue-se as instruções para concluir a instalação do Godot.

#### Configuração do Godot:

- Após a instalação, abre-se o Godot Engine.
- Depara-se com o Gerenciador de Projetos. Aqui pode-se criar um novo projeto ou abrir um existente.
- Para criar um novo projeto, clica-se em "Novo Projeto" e seleciona-se o local onde deseja salvar o projeto.
- Escolhe-se um modelo inicial para o seu projeto. Por exemplo, pode-se escolher entre 2D, 3D ou um projeto vazio.
- Depois de criar o projeto, você será levado ao editor principal do Godot.

#### Utilização do Godot:

- No editor do Godot, você pode começar a criar e desenvolver seu jogo ou aplicação.
- Use a hierarquia de cenas para organizar os elementos do seu jogo.
- Adicione novos nós às suas cenas usando o painel de nós. Existem nós para sprites, colisores, áudio, scripts, e muito mais.
- Crie scripts para adicionar comportamentos e lógica ao seu jogo. O Godot usa sua própria linguagem de script chamada GDScript, mas também suporta outras linguagens como C#.
- Teste seu jogo ou aplicação clicando no botão "Play" para visualizá-lo em ação no editor.
- Quando estiver satisfeito com o seu projeto, você pode exportá-lo para várias plataformas, como Windows, macOS, Linux, Android, iOS, HTML5, entre outras.


## Referências

- Documentação: relacionada a inspiração para o projeto:
  - [ElectroSmash](https://www.electrosmash.com/pedal-pi)
  - [NeuralPi](https://github.com/GuitarML/NeuralPi)
  - [Pi-FX : A Raspberry Pi-Based Pedal Board](https://tibbbz.medium.com/guitarix-the-pi-dle-board-8d6298ca8e42)
