### Haskell Day Firenze 26 Marzo 2016

NOTA: Il codice contenuto in questo repository non è più funzionante, per l'ultima versione vedi [quid2](https://github.com/tittoassini/quid2).

#### Semplici Sistemi distribuiti in Haskell.

Mentor: Pasqualino "Titto" Assini

Descrizione: Introduzione alla programmazione distribuita.

Livello richiesto: Da principiante ad esperto.

Il sistema che useremo è un sistema di messaggeria, in cui i messaggi sono valori di tipi definiti in Haskell.

Molto brevemente, il sistema consente di:

* definire uno o piu' tipi dati in Haskell
* inviare valori di uno qualunque dei tipi definiti
* ricevere tutti i valori di un certo tipo

E' una struttura minimalista ma molto flessibile, una sorta di grado zero dei sistemi distribuiti.

Supponiamo ad esempio di voler implementare una chat intelligente, tipo Haskel lRC.

Un semplice modello dati potrebbe essere:

```haskell
      -- Data model for a simple chat system
      data Message = Message {fromUser::User
                             ,subject::Subject
                             ,content::Content}
                   deriving (Eq, Ord, Read, Show, Generic)

      type User = String

      -- Hierarchical subject
      -- Example: Subject ["Haskell","Meeting","Firenze"]
      data Subject = Subject [String] deriving (Eq, Ord, Read, Show, Generic)

      -- Different kinds of contents
      data Content =
             -- Basic text message
             TextMessage String

             -- Some extensions that we might implement during the meeting:

             -- Retrieve the subject being discussed
             | AskSubSubjects

             -- A system to track users that are following the current subject
             | Join
             | Leave
             | AskUsers      -- Ask for list of users 
             | Users [User]  -- Return list of users

             -- Retrieve recent messages
             | AskHistory         -- Ask
             | History [Message]  -- Return last messages

             -- Haskell queries
             | HaskellSearch String
             | HaskellExpression String
```

Una volta definito il modello dati, possiamo immediatamente cominciare ad inviare a ricevere messaggi e quindi procedere a creare una semplice interfaccia utente e magari qualche (ro)bot che quando vede un valore di un tipo particolare reagisce in maniera intelligente.

Ad esempio se si tratta di una HaskellExpression la valuta e ritorna il risultato o quando vede una HaskellSearch fa una ricerca su Hoogle.

Dato che queste varie funzionalità sono indipendenti una dall'altra ognuno può scegliere il pezzo che preferisce e lavorare in parallelo, in modo da far emergere pezzo dopo pezzo un sistema più completo ed intelligente.

###### Installazione del software 

L'installazione va eseguita **prima** di arrivare al meeting, in modo da risparmiare tempo ed individuare eventuali problemi.

Prerequisiti: Un [cliente git](https://git-scm.com/) e il tool di installazione [stack](http://docs.haskellstack.org/).

Procedura:

`git clone --recursive https://github.com/tittoassini/HaskellMeetingFirenze2016.git`

`cd HaskellMeetingFirenze2016`

`stack build`

L'installazione può richiedere diversi minuti.

Per verificare che tutto funzioni, fate partire il programma `hchat` (che è l'esempio da cui partiremo al meeting):

```
stack exec hchat

Enter your name:
titto

Help:
To send a message: just enter it and press return.
To exit: Ctrl-D.

Current Subject: (Haskell/Meeting/Firenze/26 Marzo 2016/Sessione: Applicazioni Distribuite)  

Salve a tutti!
```
Fatto!

Se avete tempo, date un occhiata al codice dei programmi `hchat` ed `hchat-history` nella subdirectory `chat\app`. 

Altri comandi utili, per lo sviluppo che faremo durante il meeting:

-- Per fare un update all'ultima versione del codice:

`git pull;git submodule update --remote;stack build`

-- Per ricompilare automaticamente il codice quando viene modificato:

`stack build --file-watch`

-- Per evitare problemi con la soprapposizione dell'input dell'utente con i messaggi in arrivo, hchat può essere anche usato solo in modalità Input or Output. Ha anche una opzione Debug per mostrare esattamente che messaggi vengono scambiati:

`hchat Input`

`hchat Output`

`hchat Output Debug`

Se avete problemi, aprite un **Issue** su questo repository o scrivete a Titto a: tittoassini@gmail.com
