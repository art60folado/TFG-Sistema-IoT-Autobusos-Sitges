# TFG: Disseny i implementació d'un sistema de monitoratge IoT per a l'optimització logística dels autobusos urbans de Sitges

Un sistema d'Ajuda a l'Explotació (SAE) de baix cost basat en tecnologia Internet de les Coses (IoT) per a la monitorització i geolocalització en temps real de la xarxa d'autobusos urbans de Sitges.

Aquest projecte s'ha desenvolupat com a **Treball Final de Grau (TFG)** per a la titulació d'Enginyeria d'Organització Industrial i Logística a la **Universitat de Lleida (UdL)**. Tota la documentació acadèmica detallada i la memòria oficial es troben publicades i accessibles a través del **Repositori Obert de la UdL**.

---

## Autoria

* **Autor:** Arons Aromice Okolue
* **Institució:** Universitat de Lleida (UdL)
* **Titulació:** Grau en Enginyeria d'Organització Industrial i Logística
* **Director:** Grau Baquero Armans


---

## Descripció del Projecte

El projecte neix amb l'objectiu de resoldre la incertesa del viatger a les parades del bus urbà de Sitges mitjançant una arquitectura modular *open-source* de baix cost (inferior a 100 €). El sistema trenca la dependència amb les costoses solucions tancades industrials tradicionals i es divideix en tres capes principals:

1. **Capa de Vora (Hardware):** Un node embarcat al vehicle basat en la placa **LilyGO T-SIM7000G** (ESP32) que captura la telemetria mitjançant GNSS i la transmet via xarxa cel·lular LTE/4G.
2. **Capa de Servidor (Backend):** Una instància centralitzada en **Node-RED** que actua com a orquestrador, processa les cadenes JSON rebudes mitjançant un *broker* MQTT (QoS 0) i executa l'algorisme de predicció del Temps Estimat d'Arribada (ETA). L'entorn local s'exposa de forma pública mitjançant un túnel segur amb **Ngrok**.
3. **Capa d'Usuari (Frontend):** Una aplicació mòbil dissenyada en **FlutterFlow** que realitza consultes periòdiques (HTTP GET) a l'API del servidor per renderitzar la posició de l'autobús en un mapa interactiu i mostrar els minuts restants de pas.

---

## Contingut del Repositori

Aquest repositori conté els fitxers de codi font, configuracions de servidor i dades de camp utilitzats per a la materialització de la Prova de Concepte (MVP):

### 1. Estudi Demogràfic i de la Demanda
* **`Enquesta sobre la fiabilitat i l'ús de l'autobús urbà de Sitges (Respostes).xlsx`**: Arxiu de full de càlcul original que recull les dades brutes, el buidatge de respostes i l'anàlisi gràfica de l'enquesta de satisfacció i intenció d'ús distribuïda a la població local a través de *Google Forms*. Serveix com a justificació estadística de la viabilitat del projecte.

### 2. Firmware del Node IoT (Arduino IDE)
* **`gpstest01.ino`**: Codi de control inicial utilitzat en el laboratori per realitzar els assajos preliminars de maquinari, comprovar la connexió de les antenes, verificar el correcte arrancada dels mòduls GNSS/LTE i calibrar el format de sortida de les coordenades.
* **`LILYGOV1.1.ino`**: Script modificat i optimitzat per a l'entorn de producció i proves de camp real sobre la línia L2. Inclou la gestió asíncrona de connexió a la xarxa M2M, la serialització de trames JSON, l'empaquetat de telemetria (Latitud, Longitud, ID del bus) i la publicació automatitzada cap al *Broker* MQTT.

### 3. Servidor i Fluxos d'Orquestració
* **`flows.json`**: Fitxer d'exportació complet de la configuració de **Node-RED**. Conté la xarxa estructural de nodes del *backend*, que inclou:
  * La recepció de dades des del node *MQTT In*.
  * Els punts d'accés (*endpoints*) de l'API HTTP (Mètodes GET/POST) per servir les coordenades i els horaris estàtics a l'App.
  * L'algorisme de lògica funcional per al càlcul de l'ETA.
  * El mòdul receptor per a les notificacions de parada a la demanda.

### 4. Disseny Mecànic i Modelat 3D (FreeCAD)
* **`tapasuplilygo.FCStd`**: Fitxer font de modelat tridimensional en FreeCAD que conté el disseny paramètric de la tapa superior de l'envoltoris protector. Inclou les toleràncies d'ajust per al tancament i els guals passants dissenyats per a la sortida dels cables de les antenes actives de GPS i LTE.
* **`lilygo bottom boxv1.FCStd`**: Fitxer font de FreeCAD de la caixa o base inferior del contenidor mecànic. Està dissenyat a mida amb els ancoratges estructurals necessaris per allotjar rígidament tant la placa LilyGO T-SIM7000G com el suport de la cel·la d'alimentació 18650, actuant com a xassís antivibracions a l'interior de l'autobús.

---

## Tecnologies Utilitzades

* **Maquinari:** LilyGO T-SIM7000G (Microcontrolador ESP32, Mòdem LTE, GPS/GNSS)
* **Entorn de Desenvolupament:** Arduino IDE
* **Disseny Mecànic:** FreeCAD (Carcassa i envoltoris protectors)
* **Laminador 3D:** Creality Print (Impressió FDM en polímer PLA)
* **Servidor Logístic:** Node-RED (Node.js)
* **Protocols de Comunicació:** MQTT, HTTP REST (JSON)
* **Eina d'Exposició de Xarxa:** Ngrok
* **Disseny de la Interfície (App):** FlutterFlow
