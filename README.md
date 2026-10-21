# AA2: Avaluació i Anàlisis de Kea DHCP

## Bloc 1: Fonaments del Protocol i Anàlisi de Tràfic (Wireshark / tcpdump)

### Procés de Negociació (DORA):

Descriu la funció dels quatre missatges del cicle DHCP (Discover, Offer, Request, Acknowledge).

#### 1. Funció dels quatre missatges DHCP:
* **Discover**: El client busca servidors DHCP a la xarxa mitjançant difusió (broadcast).
* **Offer**: El servidor DHCP ofereix una adreça IP disponible al client.
* **Request**: El client sol·licita formalment l'adreça IP oferta pel servidor escollit.
* **Acknowledge (ACK)**: El servidor confirma l'assignació de l'adreça IP i envia tota la configuració de xarxa addicional.

---

### 2. Broadcast i Unicast:
Indica quines d'aquestes fases es realitzen mitjançant missatges de tipus **Broadcast** i quines en **Unicast** en un entorn local sense DHCP Relay.

* Els missatges **Discover** i **Request** són habitualment **Broadcast** (perquè el client encara no té IP o no coneix el servidor).
* Els missatges **Offer** i **Acknowledge (ACK)** poden ser **Broadcast** o **Unicast** (segons la configuració del client i si disposa o no de direcció MAC/IP temporal).

---

### Captura i Capçaleres de Paquet:
* **Especifica els ports UDP font i destí utilitzats pel client i pel servidor DHCP:**
  * El **client** utilitza el port UDP **68** (font o destí).
  * El **servidor** utilitza el port UDP **67** (font o destí).

* **Quin camp de la capçalera DHCP s'utilitza per identificar unívocament la targeta de xarxa (NIC) del client?**
  * El camp `chaddr` (Client Hardware Address), que conté l'adreça MAC de la targeta de xarxa del client.

* **Identifica el número o nom de les Opcions DHCP (DHCP Options) encarregades de transmetre la porta d'enllaç per defecte (Router), els servidors DNS i el nom de domini:**
  * **Opció 3 (`routers`)**: Porta d'enllaç per defecte (Gateway).
  * **Opció 6 (`domain-name-servers`)**: Servidors DNS primari i secundari.
  * **Opció 15 (`domain-name`)**: Nom de domini local.

---

### Captures i Evidències del Bloc 1:
![Captura Ubuntu i Xarxa](images/imagen_1_p2.png)
![Captura KEA instal·lat](images/imagen_2_p2.png)

---

## Bloc 2: Configuració del Servidor Kea DHCP (`kea-dhcp4.conf`)

Redacta l'estructura del fitxer de configuració JSON per al dimoni `kea-dhcp4` complint els requisits següents:

### 1. Interfície i Emmagatzematge:
* Configuració de la interfície de xarxa `eth0`.
* Model de base de dades de cessions en memòria (`memfile`) amb persistència activada.

### 2. Paràmetres Globals i de Temps:
* Temps de cessió (`valid-lifetime`): 2 hores (**7200 segons**).
* Temps de renovació (`renew-timer`): 1 hora (**3600 segons**).

### 3. Subxarxa i Pool Dinàmic:
* Adreça de xarxa: `192.168.x.0/24`
* Rang d'adreces dinàmiques assignables (`pool`).

### 4. Opcions de Xarxa (`option-data`):
* Encaminador / Porta d'enllaç: `192.168.x.1`
* Servidors DNS primari i secundari: `1.1.1.1` i `8.8.8.8`
* Nom de domini local: `oficina.local`

---

### Captures i Evidències del Bloc 2:
![Configuració Kea](images/imagen_3_p3.png)
![Kea Activat](images/imagen_4_p3.png)
![IP Zorin](images/imagen_5_p3.png)
![Verificació Kea](images/imagen_6_p3.png)
![Captura Bloc 2](images/imagen_7_p4.png)
![Captura Bloc 2](images/imagen_8_p4.png)
![Captura Bloc 2](images/imagen_9_p4.png)
![Captura Bloc 2](images/imagen_10_p4.png)
![Captura Bloc 2](images/imagen_11_p4.png)
![Captura Bloc 2](images/imagen_12_p5.png)
![Captura Bloc 2](images/imagen_13_p6.png)

---

## Bloc 3: Assignació Estàtica (Reserves per Adreça MAC)

Afegeix la secció de reserves (`reservations`) dins de la configuració de la subxarxa per garantir una adreça IP fixa a una impressora de xarxa de l'oficina:

* **Paràmetres de la reserva:**
  * Adreça MAC de la targeta (`hw-address`): `aa:bb:cc:11:22:33`
  * Adreça IP assignada: `192.168.x.20`
  * Nom de l'equip (`hostname`) l'impressora: `impresora-dir`

### Validació de Sintaxi:
Indica quines eines o comandes utilitza Kea o el sistema per comprovar que el fitxer JSON no conté errors de sintaxi abans d'iniciar el servei.

---

### Captures i Evidències del Bloc 3:
![Comprovació de Sintaxi i Reserves](images/imagen_14_p7.png)
![Validació sense errors](images/imagen_15_p7.png)
![Evidència Bloc 3](images/imagen_16_p8.png)
![Evidència Bloc 3](images/imagen_17_p8.png)

---

## Bloc 4: Comprovació, Diagnòstic i Resolució d'Incidències

### Comprovacions al Servidor:
* **Comandes `systemctl`:** `systemctl status kea-dhcp4-server`, `systemctl restart kea-dhcp4-server`, `systemctl stop kea-dhcp4-server`.
* **Ruta de logs i cessions:** `/var/log/kea/` i `/var/lib/kea/kea-leases4.csv`.

### Comandes en Clients:
* **Linux:** `sudo dhclient -r`, `sudo dhclient`, `nmcli device disconnect eth0`, `ip addr`.
* **Windows:** `ipconfig /release`, `ipconfig /renew`, `ipconfig`.

---

### Captures i Evidències del Bloc 4:
![Comprovacions systemctl i logs](images/imagen_18_p8.png)
![Registres i cessions](images/imagen_19_p8.png)
![Captura client Linux/Windows](images/imagen_20_p9.png)
![Captura addicional Bloc 4](images/imagen_21_p9.png)

---

## Bloc 5: Disseny del Mapa Conceptual del Servei DHCP

Mapatge conceptual complet de tots els components del servei Kea DHCP.

---

### Esquema / Captura Final del Mapa Conceptual:
![Mapa Conceptual Final](images/imagen_22_p10.png)

