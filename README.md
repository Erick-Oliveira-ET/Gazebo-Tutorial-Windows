# Instalar Gazebo no Windows usando WSL 
Se você já tem linux não siga esse tutorial - talvez a parte final de instalação já que utiliza os mesmos passos do site do gazebo. 
Se você estiver pensando "ah, tem um tutorial de como instalar gazebo no windows no site", como eu mesmo pensei, não cometa o mesmo erro que eu de tentar seguir essa forma.
É muito complicado e da muito problema. A maior parte da instalação é manual e no fim nem funcionou. Mas se quiser tentar, boa sorte.

##Habilitando WSL no windows
WSL (Windows Subsystem for Linux) é uma ferramenta do windows para a criação de um subsistema Linux dentro do Windows.
Para habilitar essa ferramenta existem duas formas:
1 .  A forma como está escrito no site da Microsoft (https://docs.microsoft.com/pt-br/windows/wsl/install-win10?redirectedfrom=MSDN), que é como eu fiz:
-  Abra o powershell como administrador
- Digite "dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart" e aperte enter
- Reinicie o computador
- Abra o powershell como administrador novamente
- Digite "wsl --set-default-version 2" e aperte enter

2 . Essa forma eu não testei, só vi que era possível:
- Clique em Win e digite 'painel de controle'
- Clique em programas - o da caixinha com o cd, onde desinstala programas
- Clique em "Ativar ou desativar recursos do Windows"
- Marque a caixinha escrita "Subsistema do Windows para Linux"
- Reinicie o computador

## Instalando e preparando o Linux (Ubuntu)
- Vá na microsoft store e procure por 'linux' ou 'wsl'- Particularmente, eu intalei o Ubuntu(O link que está no tutorial é 'https://aka.ms/wslstore')
- Depois de instalado execute o Ubuntu
- Quando aberto vai aparecer uma mensagem que diz que está instalando e que pode demorar - acredite, demora
- Quando acabar de instalar pode aparece uma mensagem de erro que diz:
 "Configurações do console incompatíveis. Para usar esse recurso, o console herdado deve ser desabilitado". Senão aparecer prossiga no tutorial.
  Se esse for o caso, é só fazer o seguinte:
 -  Clique com o botão direito do canto superior esquerdo na logo do Ubuntu
 -  Clique em propriedades
 -  Desmarque a caixa que está escrito: "Usar o console herdado"
 - Depois, marque todas as caixas acima
 - Clique em cores nas abas superiores
 - Certifique de que as cores estão corretas
 - Reinicie o Ubuntu - só fechar e abrir

- Digite um nome de usuário e senha

## Instalando Gazebo - praticamente seguir o tutorial do seguinte link:"http://gazebosim.org/tutorials?tut=install_ubuntu&cat=install":
- Primeiro verifique se há atualizações disponíveis executando "sudo apt-get update".
- Prepare o Ubuntu para aceitar softwares vindos de packages.osrfoundation.org executando:
  " sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list' "
 - Se tudo der certo, execute " cat /etc/apt/sources.list.d/gazebo-stable.list " e " deb http://packages.osrfoundation.org/gazebo/ubuntu-stable bionic main "
  vai aparecer como resposta

- Ajustando as chaves: execute " wget https://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add - ". Se você for tão sortudo quanto eu,
  a seguinte mensagem de erro apareceu: " gpg: can't connect to the agent: IPC connect call failed ".
 - Eu segui os passos descritos nesse link " https://askubuntu.com/questions/611221/gpg-error-http-packages-osrfoundation-org " e deu certo:
  - Execute os seguintes comandos:
   - wget  http://packages.osrfoundation.org/gazebo.key
   - sudo apt-key add gazebo.key

- Atualize novamente executando " sudo apt-get update && sudo apt-get upgrade " - essa atualização demora bastante, mais de 20 minutos.
- Instale o Gazebo - quando eu escrevi esse tutorial tava na versão 11: " sudo apt-get install gazebo11 " - demora mais uns 20 minutos pra mais.
 - No meu caso, apareceu a seguinte mensagem de erro: " gzclient: error while loading shared libraries: libQt5Core.so.5: cannot open shared object file: No such file or directory "
Pra solucionar esse problema eu executei: " sudo strip --remove-section=.note.ABI-tag /usr/lib/x86_64-linux-gnu/libQt5Core.so.5 ".
 Essa solução está em https://github.com/dnschneid/crouton/wiki/Fix-error-while-loading-shared-libraries:-libQt5Core.so.5

- Execute "Gazebo". Se nada acontecer é pq deu certo

## Inicializando Gazebo https://www.youtube.com/watch?v=4toU7sS0eRA&t=814s
- Instale o Xming " https://sourceforge.net/projects/xming/ ". Ele é usado pra abrir o GUI do gazebo.
- Depois de intalar clique na tecla Win e digite " Xlaunch " e execute 
- Clique em Ok e Avançar sem mudar nada até o final
- No ubunto execute " Gazebo ", depois " echo $DISPLAY ", depois " export DISPLAY=:0 " e por fim " Gazebo "
- Se tudo der certo o GUI do gazebo deve ter sido aberto

## Bugs
 - Depois da instalação o meu gazebo abriu perfeitamente. Entretanto, após tentar novamente, aparêcia uma tela preta onde deveria estar o ambiente de construção do robô. Pra resolver esse problema eu simplesmente fiz como sugerido em um issue no [github] (https://github.com/microsoft/WSL/issues/3368#issuecomment-414717437): Executei " export GAZEBO_IP=127.0.0.1 " e depois " gazebo --verbose ". Depois de abrir, eu fechei pra ver se funcionava e não funcionou. Apareceu o seguinte erro:
  X Error of failed request: BadMatch (invalid parameter attributes) Major opcode of failed request: 143 (GLX) Minor opcode of failed request: 5 (X_GLXMakeCurrent) Serial number of failed request: 20 Current serial number in output stream: 20
  Reiniciei o computador e voltou a funcionar.
  
