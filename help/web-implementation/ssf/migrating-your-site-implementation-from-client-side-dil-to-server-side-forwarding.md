---
title: Migrazione dell’implementazione Audience Manager del sito da DIL lato client a inoltro lato server
description: Scopri come migrare l’implementazione di Audience Manager (AAM) per il tuo sito da DIL lato client a inoltro lato server. Questa esercitazione si applica se disponi sia di AAM che di Adobe Analytics e se invii hit dalla pagina ad AAM utilizzando il codice di DIL (Data Integration Library), nonché se invii hit dalla pagina ad Adobe Analytics.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# Migrazione dell’implementazione Audience Manager del sito da DIL lato client a inoltro lato server {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Questo tutorial è applicabile se disponi sia di Adobe Audience Manager (AAM) che di Adobe Analytics e stai inviando un hit dalla pagina ad AAM utilizzando il codice DIL ([!DNL Data Integration Library]), nonché un hit dalla pagina ad Adobe Analytics. Poiché si dispone di entrambe queste soluzioni e poiché fanno entrambe parte di Adobe Experience Cloud, è possibile seguire la best practice per attivare l&#39;inoltro lato server, che consente ai server di raccolta dati [!DNL Analytics] di inoltrare i dati di analisi del sito in tempo reale ad Audience Manager, invece di inviare un hit aggiuntivo dalla pagina ad AAM tramite il codice lato client. Questo tutorial illustra i passaggi necessari per passare dall’implementazione lato client di DIL precedente al più recente metodo di inoltro lato server.

## Lato client (DIL) e lato server {#client-side-dil-vs-server-side}

Quando si confrontano e si confrontano questi due metodi per ottenere i dati di Adobe Analytics in AAM, potrebbe essere utile innanzitutto visualizzare le differenze nella seguente immagine:

![lato client a lato server](assets/client-side_vs_server-side_aam_implementation.png)

### Implementazione DIL lato client {#client-side-dil-implementation}

Se si utilizza questo metodo per ottenere i dati di Adobe Analytics in AAM, si otterranno due risultati dalle pagine Web: uno da [!DNL Analytics] e uno da AAM (dopo aver copiato i dati di [!DNL Analytics] nella pagina Web. [!UICONTROL Segments] vengono restituiti da AAM alla pagina, dove possono essere utilizzati per la personalizzazione e così via. Questa viene considerata un’implementazione legacy e non è più consigliata.

Oltre al fatto che non si tratta di seguire le best practice, gli svantaggi dell’utilizzo di questo metodo includono:

* Due hit provenienti dalla pagina invece di uno solo
* l&#39;inoltro lato server è necessario per la condivisione in tempo reale dei tipi di pubblico di AAM a [!DNL Analytics], pertanto le implementazioni lato client non consentono questa funzione (e potenzialmente altre funzioni in futuro)

È consigliabile passare a un metodo di inoltro lato server per l’implementazione di AAM.

### Implementazione dell&#39;inoltro lato server {#server-side-forwarding-implementation}

Come mostrato nell’immagine precedente, un hit viene dalla pagina web ad Adobe Analytics. [!DNL Analytics] inoltra quindi tali dati ad AAM in tempo reale e i visitatori vengono valutati in caratteristiche di AAM e [!UICONTROL segments], come se l&#39;hit provenisse direttamente dalla pagina.

[!UICONTROL Segments] vengono restituiti sullo stesso hit in tempo reale a [!DNL Analytics], che inoltra la risposta sulla pagina Web per la personalizzazione e così via.

Non esiste alcun downside temporale per il passaggio a Server-Side Forwarding. Adobe consiglia vivamente a tutti coloro che dispongono sia di Audience Manager che di [!DNL Analytics] di utilizzare questo metodo di implementazione.

## Hai due attività principali {#you-have-two-main-tasks}

Ci sono un bel po &#39;di informazioni su questa pagina, ed è tutto importante, naturalmente. Tuttavia, **tutto si riduce a due operazioni principali da eseguire**:

1. Modifica il codice da codice DIL lato client a codice inoltro lato server
1. Capovolgere l&#39;opzione in [!DNL Analytics] [!DNL Admin Console] per avviare l&#39;effettivo inoltro dei dati (per [!UICONTROL report suite])

Se salti una di queste attività, l&#39;inoltro lato server non funzionerà correttamente. In questo documento sono stati aggiunti passaggi e dati aggiuntivi per aiutarti a eseguire questi due passaggi correttamente per la tua configurazione.

## Opzioni di implementazione {#implementation-options}

Quando passi dall’inoltro lato client a quello lato server, una delle attività che dovrai svolgere è quella di modificare il codice per inserirlo nel nuovo codice di inoltro lato server. Questa operazione viene eseguita utilizzando una delle seguenti opzioni:

* Tag Adobe Experience Platform: opzione di implementazione consigliata da Adobe per le proprietà web. Questa è un’attività semplice, poiché i tag di Platform hanno svolto tutto il lavoro necessario.
* Sulla pagina - Puoi anche inserire il nuovo codice SSF direttamente nella funzione `doPlugins` all&#39;interno del file `appMeasurement.js`, se non utilizzi (ancora) Adobe Launch
* Altri gestori di tag: possono essere trattati come l&#39;opzione precedente (Sulla pagina), in quanto il codice SSF verrà comunque inserito in `doPlugins`, ovunque l&#39;altro gestore di tag memorizzi il codice [!DNL AppMeasurement]

Esamineremo ciascuno di questi elementi nella sezione _Aggiornamento del codice_.

## Passaggi di implementazione {#implementation-steps}

I passaggi seguenti descrivono l’implementazione.

### Passaggio 0: prerequisito: servizio Experience Cloud ID (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

Il prerequisito principale per passare all&#39;inoltro lato server è l&#39;implementazione del servizio Experience Cloud ID. Questa operazione è più semplice se utilizzi Experience Platform Launch, nel qual caso installerai semplicemente l’estensione ECID e il resto funzionerà.

Se utilizzi un TMS non Adobe, o se non utilizzi alcun TMS, implementa ECID per eseguire **prima** di qualsiasi altra soluzione Adobe. Per ulteriori dettagli, consulta la [documentazione ECID](https://experienceleague.adobe.com/docs/id-service/using/home.html). L’unico altro prerequisito è relativo alle versioni del codice, quindi puoi applicare semplicemente le versioni più recenti del codice nei passaggi seguenti.

>[!NOTE]
>
>Leggi l&#39;intero documento prima di implementare. La sezione &quot;Tempistiche&quot; seguente contiene informazioni importanti su *quando* devi implementare ogni elemento, incluso ECID (se non è ancora implementato).

### Passaggio 1: registra le opzioni attualmente utilizzate dal codice DIL {#step-record-currently-used-options-from-dil-code}

Quando ti prepari a passare dal codice DIL lato client all’inoltro lato server, il primo passaggio consiste nell’identificare tutto ciò che si sta facendo con il codice DIL, inclusi le impostazioni personalizzate e i dati inviati ad AAM. Gli aspetti da considerare includono:

* Variabili [!DNL Analytics] normali, con il modulo DIL `siteCatalyst.init` - Non è necessario preoccuparsi di questa, perché il suo lavoro consiste solo nell&#39;inviare le normali variabili [!DNL Analytics] e questo accade in virtù della semplice attivazione dell&#39;inoltro lato server.
* Sottodominio partner: nella funzione `DIL.create` prendere nota del parametro `partner`. Questo è noto come &quot;sottodominio partner&quot; o talvolta &quot;ID partner&quot; e sarà necessario quando inserisci il nuovo codice di inoltro lato server.
* [!DNL Visitor Service Namespace] - Noto anche come &quot;[!DNL Org ID]&quot; o &quot;[!DNL IMS Org ID]&quot;, ne avrai bisogno quando imposti il nuovo codice di inoltro lato server. Prendetene nota.
* containerNSID, uuidCookie e altre opzioni avanzate: prendi nota di eventuali opzioni avanzate aggiuntive in uso in modo da poterle impostare anche nel codice di inoltro lato server.
* Variabili di pagina aggiuntive - Se dalla pagina vengono inviate ad AAM altre variabili (oltre alle normali [!DNL Analytics] variabili gestite da siteCatalyst.init), dovrai prenderne nota in modo che possano essere inviate tramite inoltro lato server (avviso spoiler: tramite [!DNL contextData] variabili).

### Passaggio 2: aggiornare il codice {#step-updating-the-code}

Nelle [opzioni di implementazione](#implementation-options) (sopra) sono disponibili più opzioni relative a come e dove si implementa l&#39;inoltro lato server. Affinché questa sezione sia efficace, è necessario suddividerla in queste sezioni (con due di esse combinate). Vai al metodo di questa sezione che descrive meglio le tue esigenze.

#### Tag Adobe Experience Platform {#launch-by-adobe}

Guarda il video seguente per scoprire come spostare le opzioni di implementazione dal codice DIL lato client all’inoltro lato server in Experience Platform Launch.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;On the page&quot; (Sulla pagina) o gestore di tag non Adobe {#on-the-page-or-non-adobe-tag-manager}

Guarda il video seguente per scoprire come spostare le opzioni di implementazione dal codice DIL lato client all&#39;inoltro lato server nel codice [!DNL AppMeasurement], che risiede in un file o in un sistema di gestione tag non Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Passaggio 3: abilitazione dell&#39;inoltro (per [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Finora in questo tutorial abbiamo dedicato tutto il nostro tempo al passaggio del codice dal codice DIL lato client all’inoltro lato server. Va bene, perché è la parte più difficile. Questa sezione, anche se è molto semplice, è importante quanto l’aggiornamento del codice. Questo video illustra come riflettere lo switch che consente l’effettivo inoltro di dati da Analytics ad Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**NOTA:** Come indicato nel video, ricorda che saranno necessarie fino a 4 ore per consentire la completa implementazione dell&#39;inoltro sul backend di Experience Cloud.

## Tempistica {#timing}

Come promemoria, esistono due attività principali per passare dal DIL lato client all’inoltro lato server:

1. Aggiornamento del codice
1. Capovolgimento dell&#39;opzione in [!DNL Analytics] [!DNL Admin Console]

Ma la domanda è, quale fai per primo? Importa? Ok, scusate, erano due domande. Ma le risposte sono... dipende, e sì, *può* avere importanza. Com&#39;è per essere vago? Suddividiamola. Ma prima una domanda aggiuntiva che può venire fuori se siete una grande organizzazione con numerosi siti: Devo fare tutto in una volta? Quello è un po&#39; più facile. No. Potete farlo pezzo per pezzo.

### Un po&#39; più a fondo {#a-little-deeper-dive}

Il motivo per cui la tempistica e l&#39;ordine contano è a causa di come funziona l&#39;inoltro _realmente_, che può essere riassunto nei seguenti fatti tecnici:

* Se hai implementato il servizio Experience Cloud ID (ECID) e lo switch in [!DNL Analytics] [!DNL Admin Console] (&quot;lo switch&quot;) è attivo, i dati verranno inoltrati da [!DNL Analytics] ad AAM, anche se non hai ancora aggiornato il codice.
* Se ECID non è stato implementato, i dati non verranno inoltrati, anche se l’utente è acceso e dispone del codice di inoltro lato server.
* Il codice di inoltro lato server (nei tag Platform o sulla pagina) gestisce davvero la risposta ed è necessario per completare la migrazione.
* Ricordare che il commutatore di inoltro lato server è abilitato da [!UICONTROL report suite], ma che il codice viene gestito dalla proprietà nei tag di Platform o dal file [!DNL AppMeasurement] se non si utilizzano i tag di Platform.

### Best practice {#best-practices}

Sulla base di questi dettagli tecnici, ecco le raccomandazioni su cosa fare e quando:

#### Se NON hai ancora implementato ECID {#if-you-do-not-have-ecid-yet-implemented}

1. Capovolgere lo switch in [!DNL Analytics] per ogni [!UICONTROL report suite] che verrà abilitato per l&#39;inoltro lato server.

   1. L’inoltro non si avvia ancora perché non disponi di ECID.

1. Per sito, aggiorna il codice da DIL lato client a inoltro lato server (potrebbe essere nei tag di Platform) o sulla pagina, come descritto in un’altra sezione in precedenza).

   1. L&#39;inoltro ora scorre (come hai aggiunto ECID) e dovresti anche ricevere una risposta JSON corretta al beacon [!DNL Analytics] (per ulteriori dettagli, consulta la sezione Convalida e risoluzione dei problemi di seguito).

#### Se ECID è stato implementato {#if-you-do-have-ecid-implemented}

1. Preparare e pianificare in modo da essere pronti ad aggiornare il codice da DIL all&#39;inoltro lato server PER [!UICONTROL report suite] che verrà abilitato per l&#39;inoltro lato server:

   1. Capovolgere lo switch in [!DNL Analytics] per abilitare l&#39;inoltro lato server.

      1. L’inoltro verrà avviato perché l’ECID è abilitato.

   1. Non appena possibile, aggiorna il codice da DIL lato client a inoltro lato singolo (potrebbe essere nei tag di Platform o sulla pagina, come descritto in un’altra sezione in precedenza).

      1. Dovresti ricevere una risposta JSON corretta al beacon [!DNL Analytics] (per ulteriori dettagli, consulta la sezione [Convalida e risoluzione dei problemi](#validation-and-troubleshooting) di seguito).

>[!NOTE]
>
>È importante eseguire questi due passaggi il più vicino possibile, perché tra i passaggi 1 e 2 di cui sopra, si verificherà una duplicazione dei dati in AAM. In altre parole, l&#39;inoltro lato singolo inizierà a inviare dati da [!DNL Analytics] ad AAM e, poiché il codice DIL è ancora nella pagina, si verificherà anche un hit che andrà direttamente dalla pagina ad AAM, raddoppiando in tal modo i dati. Non appena aggiorni il codice da DIL all&#39;inoltro lato server, questo sarà alleviato.

>[!NOTE]
>
>Se preferisci una piccola discrepanza nei dati piuttosto che una piccola duplicazione di dati, puoi cambiare l’ordine dei passaggi 1 e 2 precedenti. Se si sposta il codice da DIL all&#39;inoltro lato server, il flusso di dati in AAM verrà interrotto finché non sarà possibile capovolgere lo switch per attivare l&#39;inoltro lato server per [!UICONTROL report suite]. In genere, i clienti preferiscono raddoppiare i dati in modo lieve, anziché perdere l&#39;opportunità di convertire i visitatori in caratteristiche e [!UICONTROL segments].

#### Tempistica di migrazione quando si dispone di molti siti e [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Questo argomento è trattato brevemente nelle sezioni precedenti, in quanto la strategia principale può essere riassunta come segue:

Eseguire la migrazione di un sito/[!UICONTROL report suite] (o gruppo di siti/[!UICONTROL report suites]) alla volta.

Tuttavia, questo può diventare un po’ complicato in base ad alcuni possibili scenari:

* Si dispone di un sito contenente diversi [!UICONTROL report suites] distinti
* Hai un [!UICONTROL report suite] che include diversi siti (come un [!UICONTROL report suite] globale)
* Puoi utilizzare una proprietà Platform tags per coprire più siti
* Hai diversi team di sviluppo per siti diversi

A causa di questi elementi, può diventare un po &#39;complicato. Le cose migliori che posso suggerire sono:

* Prendi un po’ di tempo per definire una strategia per la migrazione all’inoltro lato server, in base agli elementi spiegati in precedenza
* Dato che una singola proprietà nei tag di Platform (o un singolo file [!DNL AppMeasurement]) in genere è mappata a 1 o 2 [!UICONTROL report suites] distinti, è probabile che sia possibile creare un piano che funzioni su questi gruppi distinti uno per uno, aggiornando l&#39;azienda all&#39;inoltro lato server
* Se lavori con Adobe Consulting, rivolgiti a loro per quanto riguarda il piano di migrazione, in modo che possano aiutarli quando necessario

## Convalida e risoluzione dei problemi {#validation-and-troubleshooting}

Il modo principale per verificare che l’inoltro lato server sia in esecuzione, consiste nell’esaminare la risposta a qualsiasi hit di Adobe Analytics proveniente dall’app.

Se non esegui l&#39;inoltro lato server dei dati da [!DNL Analytics] ad Audience Manager, allora non esiste alcuna risposta al beacon [!DNL Analytics] (oltre a un pixel 2x2). Tuttavia, se esegui l&#39;inoltro lato server, vi sono elementi che puoi verificare nella richiesta e nella risposta di [!DNL Analytics] che ti informeranno che [!DNL Analytics] sta comunicando correttamente con Audience Manager, inoltrando l&#39;hit e ricevendo una risposta.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>Attenzione al falso esito &quot;Success&quot;. Se è presente una risposta e tutto sembra funzionare, assicurati di avere l&#39;oggetto `stuff` nella risposta. In caso contrario, è possibile che venga visualizzato un messaggio che indica `"status":"SUCCESS"`. Per quanto sembri folle, questa è la prova che NON funziona correttamente.
>
>Questo significa che hai completato l&#39;aggiornamento del codice nei tag di Platform o in [!DNL AppMeasurement], ma che l&#39;inoltro in [!DNL Analytics] [!DNL Admin Console] non è ancora stato completato. In questo caso è necessario verificare di aver abilitato l&#39;inoltro lato server in [!DNL Analytics] [!DNL Admin Console] per [!UICONTROL report suite]. Se lo hai fatto, e non sono ancora passate 4 ore, attendi, poiché potrebbe essere necessario attendere così tanto per apportare tutte le modifiche necessarie sul backend.


![falso successo](assets/falsesuccess.png)

Per ulteriori informazioni sull&#39;inoltro lato server, consulta la [documentazione](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
