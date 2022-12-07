# Visão geral
**OneShot** executa [ataque Pixie Dust](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-Offline-WPS-Attack) sem ter que mudar para o modo monitor.
# Características
  - [ataque Pixie Dust](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-Offline-WPS-Attack);
  - integrado [3WiFi offline WPS PIN generator](https://3wifi.stascorp.com/wpspin);
  - [online WPS bruteforce](https://sviehb.files.wordpress.com/2011/12/viehboeck_wps.pdf);
  - Scanner Wi-Fi com destaque baseado em iw;
# Requisitos
  - Python 3.6 e superior;
  - [suplicante Wpa](https://www.w1.fi/wpa_supplicant/);
  - [Pixiewps](https://github.com/wiire-a/pixiewps);
  - [iw](https://wireless.wiki.kernel.org/en/users/documentation/iw).
# Configurar
## Debian/Ubuntu
**Requisitos de instalação**
  ```
  sudo apt install -y python3 wpasupplicant iw wget
  ```
**Instalando o Pixiewps**

***Ubuntu 18.04 e superior ou Debian 10 e superior***
  ```
  sudo apt install -y pixiewps
  ```
 
***Outras versões***
  ```
  sudo apt install -y build-essential unzip
  wget https://github.com/wiire-a/pixiewps/archive/master.zip && descompacte master.zip
  cd pixiewps*/
  faço
  sudo make install
  ```
**Obtendo OneShot**
  ```
  CD ~
  wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
  ```
Opcional: obter uma lista de dispositivos vulneráveis a pó mágico para realçar nos resultados da verificação:
  ```
  wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
  ```
## ArchLinux
**Requisitos de instalação**
  ```
  sudo pacman -S wpa_supplicant pixiewps wget python
  ```
**Obtendo OneShot**
  ```
  wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
  ```
Opcional: obter uma lista de dispositivos vulneráveis a pó mágico para realçar nos resultados da verificação:
  ```
  wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
  ```
## Alpine Linux
Ele também pode ser usado para rodar em dispositivos Android usando [Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)

**Requisitos de instalação**
Adicionando o repositório de teste:
  ```
  sudo sh -c 'echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories'
  ```
  ```
  sudo apk add python3 wpa_supplicant pixiewps iw
  ```
  **Obtendo OneShot**
  ```
  sudo wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
  ```
Opcional: obter uma lista de dispositivos vulneráveis a pó mágico para realçar nos resultados da verificação:
  ```
  sudo wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
  ```
## [Termux](https://termux.com/)
Por favor, note que o acesso root é necessário.

#### Usando o instalador
  ```
  curl -sSf https://raw.githubusercontent.com/drygdryg/OneShot_Termux_installer/master/installer.sh | bash
  ```
#### Manualmente
**Requisitos de instalação**
  ```
  pkg install -y root-repo
  pkg install -y git tsu python wpa-suplicante pixiewps iw openssl
  ```
**Obtendo OneShot**
  ```
  git clone --depth 1 https://github.com/drygdryg/OneShot OneShot
  ```
#### Corrida
  ```
  sudo python OneShot/oneshot.py -i wlan0 --iface-down -K
  ```

# Uso
```
  oneshot.py <argumentos>
  Argumentos necessários:
      -i, --interface=<wlan0> : Nome da interface a ser usada

  Argumentos opcionais:
      -b, --bssid=<mac> : BSSID do AP de destino
      -p, --pin=<wps pin> : Use o pino especificado (string arbitrária ou pino de 4/8 dígitos)
      -K, --pixie-dust : Executa o ataque Pixie Dust
      -B, --bruteforce : executa um ataque de força bruta online
      --push-button-connect : Executa a conexão do botão WPS

  Argumentos avançados:
      -d, --delay=<n>: define o atraso entre as tentativas de pin [0]
      -w, --write : grava as credenciais do AP no arquivo em caso de sucesso
      -F, --pixie-force : Executa o Pixiewps com a opção --force (bruteforce full range)
      -X, --show-pixie-cmd: sempre imprime o comando Pixiewps
      --vuln-list=<filename> : Use arquivo personalizado com lista de dispositivos vulneráveis ['vulnwsc.txt']
      --iface-down : Desativa a interface de rede quando o trabalho é concluído
      -l, --loop: Executa em um loop
      -r, --reverse-scan : Ordem inversa das redes na lista de redes. Útil em telas pequenas
      --mtk-wifi : ativa o driver de interface MediaTek Wi-Fi na inicialização e desativa-o ao sair
                                 (para adaptadores Wi-Fi internos implementados em MediaTek SoCs). Desligue o Wi-Fi nas configurações do sistema antes de usar isso.
      -v, --verbose: saída detalhada
  ```

## Exemplos de uso
Inicie o ataque Pixie Dust em um BSSID especificado:
  ```
  sudo python3 oneshot.py -i wlan0 -b 00:90:4C:C1:AC:21 -K
  ```
Mostrar redes disponíveis e iniciar o ataque Pixie Dust em uma rede especificada:
  ```
  sudo python3 oneshot.py -i wlan0 -K
  ```
Inicie o WPS bruteforce online com a primeira metade especificada do PIN:
  ```
  sudo python3 oneshot.py -i wlan0 -b 00:90:4C:C1:AC:21 -B -p 1234
  ```
  Inicie a conexão do botão WPS:s
  ```
  sudo python3 oneshot.py -i wlan0 --pbc
  ```
## Solução de problemas
####" RTNETLINK responde: Operação não é possível devido a RF-kill"
  Apenas corra:
```sudo rfkill desbloquear wifi```
#### "Dispositivo ou recurso ocupado (-16)"
  Tente desativar o Wi-Fi nas configurações do sistema e elimine o gerenciador de rede. Como alternativa, você pode tentar executar o OneShot com o argumento ```--iface-down```.
#### A interface wlan0 desaparece quando o Wi-Fi é desativado em dispositivos Android com MediaTek SoC
  Tente executar o OneShot com o sinalizador `--mtk-wifi` para inicializar o driver do dispositivo Wi-Fi.
# Reconhecimentos
## Agradecimentos especiais
* `rofl0r` para implementação inicial;
* `Monohrom` para testes, ajuda na captura de bugs, algumas ideias;
* `Wiire` para desenvolver Pixiewps.
