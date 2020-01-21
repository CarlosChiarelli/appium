# Objetivo do documento

Este material é um guia para o setup do ambiente de configuração e uso do Appium para automação de testes móveis. Aqui você vai: 
1. Entender como funciona o Appium e como fazer mapeamento de elementos de uma aplicação;
2. Como instanciar um dispositivo Android emulado através do Android Studio; 
3. Como organizar o código de uma automação utilizando padrões de código como o PageObjects; 
4. Como escrever testes através do framework PyTest.

Muito provavelmente este documento sofrerá ajustes e complementos ao longo do tempo :)

Em breve irei disponibilizar o mesmo conteúdo em inglês e também uma solução usando o Docker, visando tornar mais prática a etapa de configuração.

Qualquer sugestão de melhoria ou correção, por favor entrar em contato <3

# Um pouco sobre Appium

Appium é uma ferramenta open-source e multi-plataforma com foco em automação de aplicações: nativas, híbridas e sites mobile para as plataformas Android e iOS.

**Página oficial:** http://appium.io

**Página oficial do repo no GitHub:** https://github.com/appium/

Nativas, híbridas e móveis? Qual a diferença entre elas?
  - **Nativas:** aquelas aplicações que foram desenvolvidas especificamente para Android ou iOS, ou seja, a partir de seus específicos SDKs.
  - **Híbridas:** aquelas que são desenvolvidas em HTML, CSS, JavaScript e que são compatíveis com qualquer plataforma (Android, iOS, Windows).
  - **Móveis:** aquelas que podemos acessar através de um link, via página web.

# Iniciando o Setup - Download

Durante o nosso workshop vamos utlizar algumas ferramentas essenciais para a prática de automação. Baixe e instale as seguintes ferramentas, que são comuns para Windows, MAC ou Linux:
  - **Appium Desktop:** é a interface da ferramenta Appium que será o foco do nosso workshop. O download está disponível aqui: https://github.com/appium/appium-desktop/releases/tag/v1.13.0 (aqui tem um acervo para vários Sistemas Operacionais. Baixe apenas aquele que for direcionado para o seu SO.)
  
  - **JDK (JAVA Development Kit):** https://www.java.com/pt_BR/download/ 

  - **Android Studio:** é um pacote do Android Studio que possibilita que possamos instaciar dispositivos móveis de várias configurações e modelos de forma emulada e em vários níveis de API. Para isso, é preciso baixar o Android Studio e, durante o setup, marcar a opção de instalar também o AVD: https://developer.android.com/studio/index.html?hl=pt-br
  
  - **IDE:**
  Escolha uma IDE de sua preferência para desenvolver os testes na linguagem escolhida. Como vamos focar em Python, sugiro PyCharm ou VSCode. Links abaixo para download:
  - PyCharm: https://www.jetbrains.com/pycharm/
  
  - VSCode: https://code.visualstudio.com/
  
# Setup - Variáveis de ambiente - Mac:

Depois de realizadas as instalações do Appium Desktop, JAVA, Android Studio e da sua IDE, é hora de setarmos as variáveis de ambiente para que seu sistema operacional identifique os processos  e as aplicações de forma mais rápida e prática.
Para isso, abra o seu terminal, identifique a localização de instalação dos pacotes e os exporte para a variável PATH.
Após identificar a localização de onde foi instalado o seu Android, copie o caminho da pasta.
Por exemplo, para macOS a localização normalmente fica em:

```bash
/Users/user_name/Library/Android/sdk
```

Então será a partir desta pasta que vamos identificar os outros caminhos necessários, como:
```bash
/Users/user_name/Library/Android/sdk/platform-tools
/Users/user_name/Library/Android/sdk/tools
/Users/user_name/Library/Android/sdk/build-tools
```

Quando você identificar estes caminhos em sua máquina, é hora de exportar esses valores para a variável PATH, como sugere o exemplo a seguir:

```bash
export ANDROID_HOME=/your/path/to/Android/sdk 
export PATH=$ANDROID_HOME/platform-tools:$PATH 
export PATH=$ANDROID_HOME/tools:$PATH 
export PATH=$ANDROID_HOME/build-tools:$PATH 
export JAVA_HOME=/your/path/to/jdk1.8.0_112.jdk/Contents/Home 
export PATH=$JAVA_HOME/bin:$PATH
```
# Setup - Variáveis de ambiente - Windows:
Após o download (link acima) e instalação do JDK do seu ambiente Windows, é hora de configurar as variáveis de ambiente. Para isso, siga as opções de menu:
1. Propriedades do Sistema >> Configurações avançadas do sistema >> Variáveis de ambiente >> Variáveis de usuário >> Novo.
2. Insira o nome da variável como "JAVA_HOME" e insira como valor a localização exata do seu arquivo jre, por exemplo, "C:\Arquivos de Programa\Java\jdk1.2.2_2\jre".
3. Na seção de variáveis de sistema, dê um clique duplo em "Path" e adicione a expressão "%JAVA_HOME%\bin". Isto significa que você está adicionando o mesmo valor criado para JAVA_HOME, só que também contuando para a pasta \bin.
4. É só clicar OK e aplicar as mudanças de configuração.

Agora, para baixar (link acima) e instalar o Android SDK, siga os passos:
1. Siga o mesmo passo #1 descrito acima até alcançar o campo de variáveis de ambiente.
2. Agora, insira o nome da variável como "ANDROID_HOME" e insira como valor a localização exata onde seu Android SDK foi instalado, por exemplo, "C:\android-sdk".
3. Agora, mais uma vez precisamos adicionar o valor da sua nova variável à sua variável global do sistema, que é o Path: "%ANDROID_HOME%\platform-tools" e também "%ANDROID_HOME\tools%".
4. É só clicar OK e aplicar as mudanças de configuração.

# Setup - Variáveis de ambiente - Linux:
A configuração de variáveis de ambiente para Linux funciona de forma muito semelhante a do Mac. Basta que vc identifique o caminho exato de instalação do JDK e do Android e aplicar (através de export) os caminhos no seu arquivo de configuração global, que neste caso é o ~/.bashrc

Por exemplo, para Linux a localização normalmente fica em:

```bash
/Users/user_name/Library/Android/sdk
```

Então será a partir desta pasta que vamos identificar os outros caminhos necessários, como:
```bash
/Users/user_name/Library/Android/sdk/platform-tools
/Users/user_name/Library/Android/sdk/tools
/Users/user_name/Library/Android/sdk/build-tools
```

Quando você identificar estes caminhos em sua máquina, é hora de exportar esses valores para a variável PATH, como sugere o exemplo a seguir:

```bash
export ANDROID_HOME=/your/path/to/Android/sdk 
export PATH=$ANDROID_HOME/platform-tools:$PATH 
export PATH=$ANDROID_HOME/tools:$PATH 
export PATH=$ANDROID_HOME/build-tools:$PATH 
export JAVA_HOME=/your/path/to/jdk1.8.0_112.jdk/Contents/Home 
export PATH=$JAVA_HOME/bin:$PATH
```

**Dica - Windows/Linux/Mac:**
Para identificar onde está a sua pasta para JAVA_HOME, é só usar o seguinte comando no terminal:
```bash
which java
```
Deverá ser retornado o caminho até seu pacote JAVA.

**Dica 2 - Linux/Mac:**
Para evitar que suas variáveis de ambiente percam os valores, salve o conteúdo da variável no seu arquivo bashrc (Linux) ou bash_profile (macOS). Após salvar os valores, não esqueça de "compilar" o arquivo para as mudanças serem refletidas:
Para macOS:
```bash
source ~./bash_profile
```

Para Linux:
```bash
source ~/.bashrc
```
___
# Instalando o Appium

A instalação do Appium é bastante simples e não requer configuração adicional - além da do Android e do JDK. Basta baixar o Appium Desktop na página oficial do Appium(como no link do começo do documento) ou via linha de comando atráves do terminal:

```bash
npm install -g appium
```
ATENÇÃO: Não instale o Appium com sudo.

**Dica - O que é npm?**
Npm é o gerenciador de downloads para pacotes node.js. 

# Como validar se tudo tá configurado ou se falta algo?

Uma funcionalidade bem legal que o Appium oferece é o pacote Appium-doctor, cuja finalidade é conferir o checklist necessário para que seu ambiente funcione. Caso algo esteja faltando, o Appium-doctor te lista exatamente o que falta. Ele também confirma o que tá configurado como esperado. Para instalá-lo, é só instalar o pacote via npm:
```bash
npm install -g appium-doctor --android
```

Estamos usando a flag **--android** para indicar a plataforma que vamos usar o Appium. Caso fóssemos usar o iOS, usaríamos a flag **--ios--**.

Depois de instalado o Appium-doctor, é só fazer a chamada via terminal:

```bash
appium-doctor
```

Abaixo segue um exemplo de como é o retorno do Appium-doctor via terminal:
```bash
➜  bin appium-doctor
info AppiumDoctor Appium Doctor v.1.10.0
info AppiumDoctor ### Diagnostic for necessary dependencies starting ###
info AppiumDoctor  ✔ The Node.js binary was found at: /usr/local/bin/node
info AppiumDoctor  ✔ Node version is 8.11.4
WARN AppiumDoctor  ✖ Xcode is NOT installed!
info AppiumDoctor  ✔ Xcode Command Line Tools are installed in: /Library/Developer/CommandLineTools
info AppiumDoctor  ✔ DevToolsSecurity is enabled.
info AppiumDoctor  ✔ The Authorization DB is set up properly.
info AppiumDoctor  ✔ Carthage was found at: /usr/local/bin/carthage
info AppiumDoctor  ✔ HOME is set to: /Users/BEZERRA
WARN AppiumDoctor  ✖ ANDROID_HOME is NOT set!
WARN AppiumDoctor  ✖ JAVA_HOME is NOT set!
WARN AppiumDoctor  ✖ adb could not be found because ANDROID_HOME is NOT set!
WARN AppiumDoctor  ✖ android could not be found because ANDROID_HOME is NOT set!
WARN AppiumDoctor  ✖ emulator could not be found because ANDROID_HOME is NOT set!
WARN AppiumDoctor  ✖ Bin directory for $JAVA_HOME is not set
info AppiumDoctor ### Diagnostic for necessary dependencies completed, 7 fixes needed. ###
```

Tudo que estiver acompanhado do símbolo **✔** significa que está instalado corretamente.
Tudo que estiver acompanhado do símbolo **✖** significa que *NÃO* está instalado ou identificado. Esses casos você deve ajustar.

O pacote do **Xcode** é específico para iOS, então, para Android, não devemos nos preocupar.

___
# Checklist

Se você chegou até aqui, significa que provavelmente o seu setup está pronto e agora você já pode usar todos os recursos do Appium! Só para checar, instalamos e configuramnos:
- Appium Desktop
- Android Studio (pacote AVD)
- JAVA
- IDE
- Configuração de variáveis de ambiente

___
# Iniciando com o Appium

Depois de tudo configurado, é hora de iniciarmos com o Appium Desktop.

___
# Comandos ADB
ADB é uma abreviação para Android Debug Brigde. Grosseiramente traduzindo, é uma ferramenta que faz uma "ponte" de comunicação entre o seu computador e o seu dispositivo móvel Android através de linhas de comando. Através do ADB, é possível que possamos manipular o dispositvo através de comandos, de forma muito prática, como:
- Instalar/desintalar aplicativos;
- Mudar configurações internas, como: tempo de desligar tela, bloqueio/desbloqueio de tela, etc.
- Habilitar/desabilitar funções de conexões, como: wifi, dados, modo avião.
- Transfência/manipulação de arquivos;
- Rebootar e desligar o dispositivo - não funciona para ligá-lo (mas isso pode ser resolvido através de frameworks).

É também possível automatizar algumas atividades de rotina combinando comandos ADB e Python Script.

**Um pouco mais sobre comandos ADB:** https://developer.android.com/studio/command-line/adb?hl=pt-br
**Um pouco Python Script:** https://realpython.com/run-python-scripts/

___
# Emulando um dispositivo Android através do Android Studio
Durante nossos estudos podemos utilizar dispositivos emulados para a realização dos nossos testes. Isso nos dá grande versatilidade pela possibilidade de escolher o dispositivo e a versão de Android que iremos utilizar. Desta forma, é possível validar o mesmo apk em cenários diversos apenas alterando configurações.

Vamos utilizar o Android Virtual Device Manager que fica no Android Studio para isso.

Alguns pontos importantes sobre este tópico:
- Em breve farei um material falando como emular um dispositivo iOS.
- Existem outras ferramentas que emulam dispositivos Androids mas, das que testei, nenhuma é tão boa quando a do Android Studio. Por esse motivo prefiro me manter nele e recomendo o uso.

___
# Tutorial 1: instalando um apk no meu dispositivo Android emulado
O primeiro de tudo é escolher algum APK disponível na Play Store para a realização dos estudos. Ultimamente tenho utilizado o APK das Casas Bahia, pois tem boa parte de seus elementos mapeados e também porque tem diversos menus, itens e uma excelente usabilidade, o que facilita no processo de aprendizado. A seguir estão os passos para você baixar uma aplicação e fazer a instação dela no seu dispositivo:
1. Escolha o APK a ser estudado;
2. Busque o APK no Google Play Store;
3. Copie o link da Play Store do apk (o link do perfil do aplicativo);
4. Cole o link do APK no Evozi (link abaixo) para fazer o download do apk escolhido;
5. Salve o apk numa pasta de sua escolha;
6. Abra o seu Android Emulado e aguarde que ele fique na tela inicial (home screen);
7. Agora arraste o APK que vc baixou para a página inicial do seu dispositivo emulado.
8. Pode abrir o apk diretamente no dispositivo :) 
8.1. Se desejar fazer via linha de comando, é só abrir o terminal na pasta que está sua aplicação e usar o seguinte comando ADB:
```bash
adb install nome-do-apk
```

**Evozi - APK Downloader:** https://apps.evozi.com/apk-downloader/
**Google Play Store:** https://play.google.com/store/apps?hl=pt_BR

___
# Tutorial 2: Identificando os elementos da aplicação
