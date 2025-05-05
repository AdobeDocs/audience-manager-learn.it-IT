---
title: Utilizza modelli lookalike per estendere l’inventario esaurito dai dati di prime parti
description: In questa esercitazione, esaminiamo i passaggi da seguire per impostare e utilizzare modelli lookalike, in modo da poter creare nuovi tipi di pubblico e venderli come estensione del segmento di conversione.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Utilizza modelli lookalike per estendere l’inventario esaurito dai dati di prime parti {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

In questo tutorial verranno descritti i passaggi da eseguire per impostare e utilizzare Look-Alike [!UICONTROL Models], in modo da poter creare nuovi tipi di pubblico lookalike e venderli come estensione del segmento di conversione.

## Dettagli del caso d’uso {#use-case-details}

Sei un editore di contenuti. Se sul tuo sito hai già esaurito le scorte per i convertitori, potresti pensare che l’opportunità finisca lì. Immettere [!UICONTROL Models] lookalike AAM. Utilizzando questa funzione, puoi estendere ulteriormente l’inventario esaurito e vendere anche tipi di pubblico di persone che forse non si sono ancora convertite, ma che assomigliano o si comportano come persone convertite. Questo segmento di pubblico viene in genere venduto a un prezzo inferiore a quello dei convertitori effettivi, ma consente comunque di aggiungere valore al risultato finale fornendo un’opzione di pubblico aggiuntiva per gli inserzionisti che desiderano inserire annunci sul sito. Il vantaggio aggiuntivo di questo caso d’uso è che l’esecuzione di questo modello sui dati di prime parti non costa nulla.

I passaggi di questa esercitazione sono i seguenti:

1. Identificare/creare una caratteristica o un segmento utente (conversione) ideale
1. Crea un modello utilizzando questa caratteristica di conversione/segmento come elemento base
1. Scegli [!UICONTROL First party] origini dati nel modello ed esegui il modello
1. Crea un [!UICONTROL Algorithmic Trait] dai risultati del modello e aggiungi la caratteristica a un segmento
1. Offri il segmento agli inserzionisti interessati per estendere le vendite del segmento di conversione

## Identificare o creare una caratteristica o un segmento utente (conversione) ideale {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di convincere le persone a fare sul tuo sito? Qual è il tuo evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e degli obiettivi organizzativi. In ogni caso, è comune in AAM creare una caratteristica per i visitatori che hanno soddisfatto tali criteri.

In questo caso d’uso, si presume già che ciò sia avvenuto, perché l’inventario è stato esaurito per le persone che sono trasformatori. Tuttavia, ai fini di questa esercitazione, è opportuno discuterne come riferimento per il resto del caso d’uso.

Inoltre, quando si utilizzano gli eventi per creare le caratteristiche, è necessario tenere presente una caratteristica principale, in modo da non raccogliere più utenti di quanto si dovrebbe per la caratteristica. Guarda il video seguente per la grande rivelazione. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l&#39;esempio che mostro presuppone che tu disponga di Adobe Analytics. Ovviamente, questo potrebbe non essere il caso. Se hai Google Analytics (GA), abbiamo un modulo che puoi utilizzare per inviare dati all&#39;AAM (consulta la [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=it)) e se l&#39;attività di conversione sul tuo sito viene inviata all&#39;AAM da GA, puoi creare la caratteristica di conversione da tale modulo. Se disponi di una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare dati all’AAM tramite il nostro codice DIL e la funzione `submit`, ecc. (consulta la [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=it)). Quindi, di nuovo, crea la caratteristica di conversione in base ai dati inviati quando l’attività di conversione viene eseguita sul sito.

## Creare un modello lookalike dai dati di prime parti {#creating-a-look-alike-model-from-first-party-data}

In questo passaggio verrà creato un modello lookalike [!UICONTROL First Party]. Ciò significa che non solo utilizzeremo una caratteristica/segmento di conversione di prime parti per la caratteristica/segmento di base (sarebbe comunque normale per la maggior parte dei modelli), ma che esamineremo anche il pool di dati di prime parti per più persone che assomigliano ai convertitori. Non esamineremo dati di seconde o terze parti.

In questo caso d’uso, questo è importante, perché stiamo cercando di creare sul nostro sito un segmento di utenti che sembrano convertitori ma non si sono ancora convertiti, in modo da poter vendere questo segmento simile agli inserzionisti interessati.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Creare una caratteristica algoritmica {#creating-an-algorithmic-trait}

Successivamente, sarà necessario creare un [!UICONTROL Algorithmic Trait], in modo da poter utilizzare i risultati del modello. Senza creare una caratteristica, il modello è inutile. Quindi, dopo l&#39;esecuzione del modello, assicurarsi di passare alla finestra di dialogo delle caratteristiche e creare un [!UICONTROL Algorithmic Trait]. Il seguente video illustra e fornisce alcuni suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Offri [!UICONTROL Algorithmic Segment] agli inserzionisti {#offering-the-algorithmic-segment-to-advertisers}

Dopo aver creato un [!UICONTROL Algorithmic Trait], puoi creare un nuovo segmento in cui inserirlo, in modo da poter attivare i dati (non puoi attivare una caratteristica, ma piuttosto creare un nuovo segmento con una caratteristica in esso [!UICONTROL Algorithmic Trait], in modo da poter attivare (utilizzare) il segmento.

Dopo aver creato un segmento di visitatori di prime parti con un punteggio alto nel modello lookalike (ovvero che sembrano convertitori ma non si è ancora verificata una conversione), puoi offrire questo segmento agli inserzionisti sul sito, anche dopo aver esaurito tutto l’inventario dei convertitori effettivi sul sito. Questo è un ottimo modo per estendere questo pubblico e continuare a vedere ricavi aggiuntivi utilizzando Look-Alike [!UICONTROL Models] in Audience Manager.
