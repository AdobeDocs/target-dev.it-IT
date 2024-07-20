---
keywords: implementa, implementa, whitelist, whitelist, inserisce nell'elenco Consentiti di, elenco consentiti, edge, edge, 9 $
description: Visualizza un elenco di host per aiutarti a inserire nell'elenco Consentiti [!DNL Adobe Target] gli spigoli (nodi di servizio geograficamente distribuiti che garantiscono tempi di risposta ottimali agli utenti finali).
title: Inserire nell'elenco Consentiti Come posso  [!DNL Target] nodi Edge?
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 49b6572c0d414ab304712691c97794bb0b1e3781
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# Inserire nell&#39;elenco Consentiti [!DNL Target] nodi edge

Informazioni e un elenco aggiornato di host per aiutarti a inserire nell&#39;elenco Consentiti i bordi di [!DNL Adobe Target] di cui hai bisogno.

Un perimetro è un’architettura di servizio geograficamente distribuita che garantisce agli utenti finali che richiedono contenuti tempi di risposta ottimali, indipendentemente dall’area geografica in cui si trovano. Ogni nodo perimetrale dispone di tutte le informazioni necessarie per rispondere alla richiesta di contenuto dell’utente e per tenere traccia dei dati di analisi relativi a tale richiesta. Le richieste degli utenti vengono indirizzate al nodo edge più vicino. Per ulteriori informazioni, vedere [La rete Edge](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934).

Inserire nell&#39;elenco Consentiti Se necessario, puoi [!DNL Target] nodi edge.

>[!IMPORTANT]
>
>Oltre a inserire nell&#39;elenco Consentiti gli indirizzi IP NAT (Network Address Translation) di [!DNL Target] edge e [!DNL Target] indirizzi IP edge di cui all&#39;articolo, è necessario anche inserire nell&#39;elenco Consentiti tutti i blocchi di indirizzi IP di [!DNL Adobe Analytics].
>
>Per ulteriori informazioni, consulta [Tutti i blocchi di indirizzi IP di Adobe Analytics](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank} nella *note tecniche di Adobe Analytics* documentazione.
>
>È in corso l&#39;aggiornamento dell&#39;infrastruttura [!DNL Adobe Target] e i clienti che desiderano inserire nell&#39;elenco Consentiti gli indirizzi devono utilizzare entrambi i set di IP. In caso contrario, i clienti che utilizzano implementazioni lato server o ibride in cui le chiamate API di Target per il recupero delle esperienze hanno origine dall’interno di una rete dietro un firewall configurato per l’utilizzo di un elenco Consentiti di.
>
>Tutti gli indirizzi Edge4 *x* elencati in entrambe le tabelle seguenti sono pianificati per l’aggiornamento il 9 agosto 2023.

## Indirizzi IP NAT (Network Address Translation) di [!DNL Target] reti Edge

Elenco di indirizzi IP in uscita di [!DNL Target] Edge. Inserire nell&#39;elenco Consentiti questi IP se si prevede di avere [!DNL Target] accesso ai propri servizi.

| Posizione Edge | Indirizzi IP in uscita |
| --- | --- |
| Edge41 (Mumbai) | 3.6.2.221<br />13.235.112.4 <br />52.66.66.192 |
| Edge42 (Tokyo) | 52.69.55.232<br />43.206.61.43 <br />13.113.73.214 |
| Edge44 (costa orientale USA) | 54.164.192.223<br />52.86.86.203 <br />54.88.167.98 |
| Edge45 (costa occidentale degli Stati Uniti) | 52.40.124.129<br />54.148.219.69 <br />54.189.208.212 |
| Edge46 (Sydney) | 54.253.144.4<br />54.66.198.142 <br />13.211.218.51 |
| Edge47 (Irlanda) | 52.208.136.136<br />54.170.28.19 <br />99.80.111.82 |
| Edge48 (Singapore) | 3.1.141.36<br />18.143.112.116 <br />52.76.61.44 |

## [!DNL Target] indirizzi IP edge

Elenco di indirizzi IP di [!DNL Target] Edge. Se desideri effettuare chiamate API a [!DNL Target] Edge, Inserisce nell&#39;elenco Consentiti questi IP.

Questo elenco verrà modificato spesso, man mano che i load balancer aumentano o diminuiscono in base ai profili di traffico.

| Posizione Edge | Dominio | Indirizzi IP |
| --- | --- | --- |
|  | `CLIENTCODE.tt.omtrdc.net`<br />(dove CLIENTCODE è l&#39;ID client [!DNL Target]) |  |
| Edge41 (Mumbai) | `mboxedge41.tt.omtrdc.net` | 15.206.104.6<br />3.109.14.178 <br />13.234.139.131 |
| Edge42 (Tokyo) | `mboxedge42.tt.omtrdc.net` | 52.194.84.34<br />3.115.158.39 <br />18.180.123.21 |
| Edge44 (costa orientale USA) | `mboxedge44.tt.omtrdc.net` | 54.205.210.54<br />23.20.189.8 <br />35.169.173.155 |
| Edge45 (costa occidentale degli Stati Uniti) | `mboxedge45.tt.omtrdc.net` | 35.161.163.45<br />44.230.114.101 <br />35.161.120.22 |
| Edge46 (Sydney) | `mboxedge46.tt.omtrdc.net` | 3.104.142.61<br />52.62.4.152 <br />54.253.105.140 |
| Edge47 (Irlanda) | `mboxedge47.tt.omtrdc.net` | 18.203.168.186<br />54.228.83.91 <br />54.217.181.83 |
| Edge48 (Singapore) | `mboxedge48.tt.omtrdc.net` | 54.179.6.70<br />13.215.150.94 <br />18.136.47.70 |

Man mano che i load balancer rilevano le modifiche nel profilo di traffico, questo si espande verso l’alto o verso il basso. Il tempo necessario per la scalabilità di Elastic Load Balancing può variare da 1 a 7 minuti, a seconda delle modifiche rilevate. Quando i load balancer vengono scalati, aggiornano il record DNS con il nuovo elenco di indirizzi IP. Per garantire che si stia sfruttando la maggiore capacità, il bilanciamento del carico elastico utilizza un’impostazione TTL sul record DNS di 60 secondi.
