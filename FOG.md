# Instal·liació i configuració FOG

## 1. Introducció
En tecnologia de la informació, **FOG** fa referència a una arquitectura computacional que estén el concepte de **Cloud (núvol)** cap a l'extrem de la xarxa, més a prop dels dispositius i sensors. 

La idea darrere del fog computing és descentralitzar el processament i emmagatzematge de dades a la xarxa, portant-lo més a prop dels dispositius i usuaris finals. Mentre que la computació en el núvol es focalitza en processar i emmagatzemar dades en servidors remots, el fog computing permet realitzar tasques de processament, anàlisi i emmagatzematge en nodes més propers als dispositius. Això pot millorar la latència, reduir la càrrega en la xarxa i proporcionar una major autonomia en entorns amb connexió intermitent a internet. 

## 2. Procediment
### 2.1 Instal·lar i configurar un servidor FOG
Primer que tot crearem una maquina virtual anomenada **FOG**, la guardarem en la ruta **Z:\Maquina Virtual**, utilitzarem una iso d’**Ubuntu-22.04-live-server** que previament hem descarregat. Establertes les dades inicials anirem al següent pas fent clic a **Next**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_01.png)

Com a parametres de la maquina establirem:
> - Memoria RAM: 2048 bytes
> - Processadors: 2
> - Mida del disc: 152 GB

Per acabar finalitzarem el procés de creació fent clic a **Terminar**

![Imatge no es troba](./assets/images/instal%C2%B7lar_fog/captura_02.png)

Iniciarem la maquina virtual i establirem les dades per al nostre perfil:
> - Nom: fogStevenSebastian
> - Nom servidor: fog
> - Nom usuari: fogss

![Imatge no es troba](./assets/images/instal%C2%B7lar_fog/captura_03.png)

Instal·larem el servidor **OpenSSH** per a treballar de forma remota des del nostre PC

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_04.png)

Un cop hem acabat el procés d'instal·lació de la nostra maquina instal·larem el paquet git. 

Ordre utilitzada: `apt install -y git`

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_06.png)

Seguidament clonarem el repositori anomenat **fogproject** del proyecto FOG en GitHub.

Ordre utilitzada: `git clone https://github.com/FOGProject/fogproject.git` 

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_07.png)

Ens dirigirem al directori **~/fogproject/bin** i executerem l'instal·lador del FOG

Ordre utilitzada `./installfog.sh`

Seleccionarem la versió de linux: *Debian Based Linux*

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_08.png)

Parametres d'instal·lació *part1*:
> - Tipus d'instal·lacío **Normal**
> - Deixarem la interficie per defecte que ha detectat, en el nostre cas **enp0s3**
> - Establirem una adreça de router per al nostre servei DHCP
> - Deixarem que el DHCP gestione automaticament el DNS
> - No utilitzarem el servidor FOG per al servei DHCP
> - No instal·larem paquets adicionals

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_09.png)

Parametres d'instal·lació *part2*:
> - Instal·larem paquets de llenguatges adicionals
> - No habilitarem el HTTPS al nostre servidor FOG
> - Utilitzarem el nom de host per defecte que es **fog**

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_10.png)

Parametres d'instal·lació *part3*:
> - Acceptarem l'enviament del nom del sistema operatiu, la versió i la versió del FOG

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_11.png)

Revisarem que tots els parametres siguin correctes i continuarem en l'instal·lació

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_12.png)

Esperarem a que acabe el procés d'instal·lació, i ens donara un avis de que hem d'anar a la direcció **http://192.168.1.23/fog/management** per a instal·lar la base de dades, un cop la tinguem instal·lada presionarem la tecla *Enter*

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_13.png)

Un cop estem en la direcció donada anteriorment farem clic sobre el botó **Install/Update Now** per a instal·lar la base de dades

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_14.png)

Esperem uns minuts, i quan acabe l'instal·lació ens donara un missatge de *Install/Update Succesful*, és a dir, que la instal·lació s'ha fet correctament

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_15.png)

Tornarem al terminal, i ara ja presionarem la tecla *Enter*, esperarem uns segons i ja haurem acabat l'instal·lació

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_16.png)

Com podem veure en la captura anterior al instal·lar-se la base de dades ens dona l'opció de fer clic a un enllaç per a iniciar sessió, entrarem a l'enllaç i iniciarem sessió en el nostre usuari i contrasenya

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_17.png)

Un cop iniciada la sessió ja estarem dins del panel de control del FOG

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_18.png)

Tornarem al terminal i deshabilitarem el procés **system-resolved**, l'aturarem, esboorrarem l'arxiu **/etc/resolv.conf** i el tornarem a crear en el servidor DNS de google (8.8.8.8)

Ordres utilitzades:

`systemctl disable system-resolved`

`systemctl stop system-resolved`

`rm /etc/resolv.conf`

`echo "nameserver 8.8.8.8" > /etc/resolv.conf`

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_19.png)

Instal·lem el paquet **dnsmasq** que ens servirà per al direccionament dels clients al nostre FOG

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_20.png)

Mentre estem instal·lan el paquet ens sortirà una pestanya on podrem seleccionar quins paquets reiniciar, nosaltres dixarem els seleccionats per defecte.

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_21.png)

Un cop tenim instal·lat el paquet crearem l'arxiu **/etc/dnsmasq/ltsp.conf**, entrarem a editar-lo i afegirem unes linies per a establir el port en què el servidor dnsmasq escolta per les sol·licituds DNS a zero (0), registrarem els missatges relacionats amb el DHCP al registre del sistema, especificarem el directori arrel per al servidor TFTP, impedirem que les sol·licituds DHCP enviades per clients DHCP especificin els seus propis servidors DNS i porta de porta d'enllaç per substituir els valors configurats al servidor dnsmasq, especificarem la configuració de boot per als clients DHCP, configurarem el missatge que es mostra als clients DHCP durant la fase de boot PXE  i defineirem l'interval d'adreces IP que s'assignaran als clients DHCP.

Ordre utilitzada:
`nano /etc/dnsmasq/ltsp.conf`

Configuració arxiu /etc/dnsmasq/ltsp.conf: 

> - port=0
> - log-dhcp
> - tftp-root=/tftpboot
> - dhcp-no-override
> - dhcp-boot=undionly.kpxe,,192.168.1.23
> - pxe-prompt="Booting FOG Client", 1
> - dhcp-range=192.168.1.23,proxy

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_22.png)

Guardarem els canvis en l'arxiu anterior i reiniciarem el servei **dnsmasq**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_fog/captura_23.png)


### 2.2 Capturar des del client un Windows
Primer que tot instal·larem una maquina **Windows 10 Professional**.
![Imatge no es troba](/assets/images/capturar_windows/captura_01.png)

Establirem un usuari acord al nostre projecte en que simulem ser un tecnic que vol muntar un *Servidor FOG* per a **Microsoft**
![Imatge no es troba](/assets/images/capturar_windows/captura_02.png)

Un cop instal·lada la maquina descarregarem diverses aplicacions que vulguem que els nostres clients tinguen instal·lades per defecte
![Imatge no es troba](/assets/images/capturar_windows/captura_03.png)

Comprovem que tenim les aplicacions instal·lades i que el fons de l'escriptori s'hagi canviat.
![Imatge no es troba](/assets/images/capturar_windows/captura_04.png)

Anirem a l'apartat **Image Management** i seleccionarem l'opció **Create New Image**.
![Imatge no es troba](/assets/images/capturar_windows/captura_05.png)

Deixarem la majoria dels parametres per defecte, nosaltres establirem el nom de la imatge, una descripció i el sistema operatiu. Un cop acabat afegirem la imatge
![Imatge no es troba](/assets/images/capturar_windows/captura_06.png)

Un cop afegida ja podrem veure la nostra imatge en el **Image Management**.
![Imatge no es troba](/assets/images/capturar_windows/captura_07.png)

Iniciem la maquina de Windows que hem instal·lat en pasos anteriors, i entrem en F12 per a entrar a la *selecció de discos*

![Imatge no es troba](/assets/images/capturar_windows/captura_08.png)

Arrancarem per xarxa i ens connectarem per la LAN al FOG

![Imatge no es troba](/assets/images/capturar_windows/captura_09.png)

Un cop ja hem accedit al FOG registrarem el host.

![Imatge no es troba](/assets/images/capturar_windows/captura_10.png)

Esperarem el procés de registre del host

![Imatge no es troba](/assets/images/capturar_windows/captura_11.png)

Un cop registrat el host, anem al partat **Host Management** del FOG, i podrem veure com se'ns ha quedat registrat el nostre host.

![Imatge no es troba](/assets/images/capturar_windows/captura_12.png)

Entrarem a editar el host, i canviarem el nom del host, afegirem una descripció i enllaçarem la imatge que hem creat anteriorment amb aquest host.

![Imatge no es troba](/assets/images/capturar_windows/captura_13.png)

Anirem a afegir una nova tasca de captura al host.

![Imatge no es troba](/assets/images/capturar_windows/captura_14.png)

Crearem la tasca per a que quan el host entri per xarxa comenci a capturar de forma automàtica.

![Imatge no es troba](/assets/images/capturar_windows/captura_15.png)

Si tota la tasca ha sigut correctament processada ens deixara guardar-la sense problemes.

![Imatge no es troba](/assets/images/capturar_windows/captura_16.png)

Iniciem el client per a començar a fer la clonació.

![Imatge no es troba](/assets/images/capturar_windows/captura_17.png)

I clonem la imatge de la maquina **Windows 10 Professional**, un cop arriba al 100% podrem veure un missatge *Clonat correctament*.

![Imatge no es troba](/assets/images/capturar_windows/captura_18.png)

Al **Task Management** podem veure com s'esta carregant la clonació que acabem de fer.

![Imatge no es troba](/assets/images/capturar_windows/captura_19.png)

I des de **Image Management**, aqui podrem veure la imatge clonada correctament, això ho podem comprovar en la mida de la imatge.

![Imatge no es troba](/assets/images/capturar_windows/captura_20.png)

### 2.3 Capturar des del client un Ubuntu
Primer crearem la maquina **Ubuntu 22.04**, i tal com hem fet en Windows crearem l'usuari *MicrosoftUser*.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_01.png)

Descarreguem uns paquets d'algunes aplicacions que vulguem que les nostres maquines Ubuntu de l'empresa tinguin instal·lades per defecte.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_02.png)

Instal·larem cada un dels paquets que hem descarregat en el pas anterior.

Ordre utilitzada:
`sudo dpkg -i nom_paquet`

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_03.png)

Un cop instal·lades ho comprovem, i també comprovem el canvi en el fons de l'escriptori

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_04.png)

Des del FOG anem a l'apartat de **Image Management**, i creem una nova imatge.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_05.png)

Establirem el nom de la imatge, una descripcio, el sistema operatiu i la ruta de la imatge.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_06.png)

Iniciem la maquina d'**Ubuntu 22.04** i entrem a la seleccio de discos presionant **F12**.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_07.png)

Arrancarem per xarxa i ens connectarem per la LAN al FOG

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_08.png)

Un cop ja hem accedit al FOG registrarem el host.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_09.png)

Esperarem el procés de registre del host

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_10.png)

Un cop registrat el host, anem al partat **Host Management** del FOG, i podrem veure com se'ns ha quedat registrat el nostre host.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_11.png)

Entrarem a editar el host, i canviarem el nom del host, afegirem una descripció i enllaçarem la imatge que hem creat anteriorment amb aquest host.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_12.png)

Anirem a afegir una nova tasca de captura al host.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_13.png)

Crearem la tasca per a que quan el host entri per xarxa comenci a capturar de forma automàtica.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_14.png)

Si tota la tasca ha sigut correctament processada ens deixara guardar-la sense problemes.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_15.png)

Iniciem el client per a començar a fer la clonació.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_16.png)

I clonem la imatge de la maquina **Ubuntu 22.04**.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_17.png)

Al **Task Management** podem veure com s'esta carregant la clonació que acabem de fer.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_18.png)

I des de **Image Management**, aqui podrem veure la imatge clonada correctament, això ho podem comprovar en la mida de la imatge.

![Imatge no es troba](/assets/images/capturar_ubuntu/captura_19.png)

### 2.4 Instal·lar imatge Windows
El primer pas serà crear la maquina virtual **Windows10_Client_FOG**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_windows/captura_01.png)

Seguidament accedirem al FOG, mitjançant el selector de discos i conectan-nos per LAN. Un cop dins seleccionarem l'opció de **Deploy Image**

![Imatge no es troba](/assets/images/instal%C2%B7lar_windows/captura_02.png)

Iniciarem en l'usuari i contrasenya del **Servidor FOG**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_windows/captura_03.png)

Un cop iniciat seleccionam la imatge de **Windows10**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_windows/captura_04.png)

Esperarem a que acabe el proces de desplegament de la imatge sobre el nostre client Windows.

![Imatge no es troba](/assets/images/instal%C2%B7lar_windows/captura_05.png)

Un cop acabat el procés anterior, i ja reiniciada la maquina, podrem iniciar al client windows en l'usuari **Microsoft_User** que ja hem creat anteriorment.

![Imatge no es troba](/assets/images/instal%C2%B7lar_windows/captura_06.png)

Al iniciar sessió podrem veure com tenim instal·lades les aplicacions que voliem i tenim el fons de pantalla establert correctament.

![Imatge no es troba](/assets/images/instal%C2%B7lar_windows/captura_07.png)

### 2.5 Instal·lar imatge Ubuntu
Creem la maquina virtual **Ubuntu22_Client_FOG**, anirem al selector de discos, i arrancarem per LAN per accedir al FOG. Un cop hem accedit seleccionarem l'opció de **Deploy Image**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_ubuntu/captura_01.png)

Introduim l'usuari i contrasenya del FOG.

![Imatge no es troba](/assets/images/instal%C2%B7lar_ubuntu/captura_02.png)

Seleccionem la imatge d'**Ubuntu22**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_ubuntu/captura_03.png)

Esperarem a que acabe el proces de desplegament de la imatge sobre el nostre client Ubuntu.

![Imatge no es troba](/assets/images/instal%C2%B7lar_ubuntu/captura_04.png)

Un cop acabat el procés anterior, i ja reiniciada la maquina, podrem iniciar al client ubuntu en l'usuari **Microsoft_User** que ja hem creat anteriorment.

![Imatge no es troba](/assets/images/instal%C2%B7lar_ubuntu/captura_05.png)

Al iniciar sessió podrem veure com tenim instal·lades les aplicacions que voliem i tenim el fons de pantalla establert correctament.

![Imatge no es troba](/assets/images/instal%C2%B7lar_ubuntu/captura_06.png)
![Imatge no es troba](/assets/images/instal%C2%B7lar_ubuntu/captura_07.png)


### 2.6 Llençar un paquet per a que s’instal·li als clients
Primer, dins del FOG anirem a l'apartat **Snapin Management** i crearem un nou complement.

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_01.png)

Establirem un nom per al complement, una descripció, una plantilla, i afegirem l'arxiu **.msi** de **Firefox**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_02.png)

Podem comprovar que realment s'ha afegit el complement en la llista de complements

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_03.png)

Iniciem el client de windows, ens connectem al fog i seleccionem l'opcio de **Quick Registration and Inventory** per a registrar el client com a host

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_04.png)

Anem al fog i comprovem que que s'ha registrat correctament

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_05.png)

Entrem a editar el host, i fem que pugui veure els complements que li podem afegir, mirem la llista i afegim el complement de Firefox que hem creat anteriorment

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_06.png)

Confirmem la tasca

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_09.png)

I podrem veure que la tasca s'està carregant correctament

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_10.png)

Anem al client windows, ens connectem al fog per la ip, i seleccionem l'opció  **FOG Client**

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_11.png)

Ens portara a una pestanya en diferents tiputs d'instal·ladors per al client, nosaltres seleccionarem **Smart Installer (Recommended)** (Instal·lació intel·ligent).

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_12.png)

Ens obrira una pestanya on haurem de ficar la ip del servidor FOG, un cop ficada fem clic sobre el boto **Netxt**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_13.png)

Seguirem els pasos fins a completar l'instal·lació, un cop finalitzada farem clic sobre el boto **Finish**.

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_14.png)

Reiniciem el client.

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_15.png)

Podrem veure a les tasques, com tenim iniciat el **FOG Client**

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_16.png)

En notificacions podem veure com ens informa de que Firefox s'està instal·lnt i que no hem d'apagar el client mentres, després una altra notificació que ens informa de que ja s'ha instal·lat correctament

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_17.png)

El client del FOG ens informa que hem de reiniciar el sistema per a aplicar els canvis.

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_18.png)

Un cop reiniciada la nostra maquina ja tindrem instal·lada i completament operativa l'aplicació de Firefox

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_19.png)

Podrem veure que també ens guarda un log del FOG, aquí podem veure els passos que ha seguit per a afegir el complement al nostre Windows

![Imatge no es troba](/assets/images/instal%C2%B7lar_paquet/captura_20.png)


## 3. Conclusions

En resum, el projecte de fog ha demostrat ser una solució prometedora per abordar els desafiaments de la quantitat creixent de dispositius connectats i el processament de dades en temps real. La implementació amb èxit de l'arquitectura de fog ha brindat una major eficiència, menor latència i més seguretat en comparació amb els enfocaments tradicionals de computació al núvol.

## 4. Webgrafia

[Instal·lar i configurar FOG](https://youtu.be/1niqOj-RoQI)

[Clonació d'equips](https://youtu.be/rge9_wKc01c)

[Afegir complements al Client](https://youtu.be/Y3uP0OoTlfk)
