# DALI-postOffice

Il progetto è volto a gestire la coda negli uffici postali.

## Installation
### OS X & Linux:

Per scaricare e installare SICStus Prolog (è necessario), seguire le istruzioni al link https://sicstus.sics.se/download4.html.
Adesso è possibile clonare il repository ed eseguire il comando per far partire il programma:

     git clone https://github.com/Laudie/DALI-postOffice/
     cd DALI-project/application/
     bash startmas.sh

Le finestre che si apriranno:
     Prolog LINDA server (active_server_wi.pl)
     Prolog FIPA client (active_user_wi.pl)
     1 instanza di DALI per ogni agente (active_dali_wi.pl)

## Organizzazione delle cartelle

La cartella DALI-project contiene due sottocartelle application e src. Per poter eseguire il progetto è necessario andare in application e da li far partire il termina con il seguente comando 'bash startmas.sh'.

### Cartella MAS
Nella cartella mas in application/mas abbiamo gli agenti del programma.
In particolare abbiamo gli agenti Cliente che si recano alle poste perchè devono fare una certa operazione (Spedizione/Ritiri oppure Altre Operazioni). Da qui, abbiamo sia l'agente che gestirà le Spedizioni e/O Ritiri che l'agente per le Operazioni Varie. Un cliente arrivato in ufficio postale sarà munito di un biglietto utile per mettersi in fila ed attendere il proprio turno. Il biglietto sarà consegnato dall'agente Biglietteria.



## Esecuzione del programma
Il programma parte da una chiamata esterna da parte dell'utente verso l'agente Biglietteria con dei numeri interi in input che rappresentano i biglietti da consegnare ad ogni agente.
la Biglietteria a questo punto sveglia tutti gli agenti che devono fare una certa operazione assegnando i biglietti per potersi mettere in fila. L'agente Cliente salva il proprio Id e il biglietto assegnato e aspetta il proprio turno. Saranno gli agenti Spedizione e OperazioniVarie a gestire la fila secondo il numero di biglietto di ciascun Cliente. 

All'inizio si esegue il comando send_message(distribuisci_biglietti(e1,e2,e3,e4,e5,e6),user) dall'utente verso agentBiglietteria. 

## Screenshots dell'esecuzione
Nella prima schermata si vedono i 6 agenti. In particolare agentCliente1, agentCliente2 e agentCliente3 hanno come operazione Spedizione e saranno quindi gestiti dall'agente agentSpedizione. Dall'altra parte agentiCliente4,agentCliente5 e agentCliente6 saranno gestiti dall'agentOperazioniVarie. Ogni agente stampa un messaggio per far sapere cosa sta facendo e per avvisare quando l'operazione è terminata.
Nell'altra schermata abbiamo active_user_wi.pl da dove parte l'esecuzione dopo l'invio di un messaggio all'agentBiglietteria. L'agentBiglietteria stampa i biglietti assegnati ai clienti con il loro id e sveglia tutti i clienti. Gli agentOperazioniVarie e agentSpedizione gestiscono i clienti in fila e stampano dei messaggi per avvisare chi è il prossimo cliente ad entrare.

<img width="1437" alt="Schermata 2022-07-25 alle 15 03 39" src="https://user-images.githubusercontent.com/38522985/180869354-b22d5d5d-c7ca-4b03-b659-ceecc764d6d9.png">

<img width="977" alt="Schermata 2022-07-25 alle 15 02 40" src="https://user-images.githubusercontent.com/38522985/180869323-9f37ea85-f3f1-4441-8ff5-345665218d5e.png">


## Sequence Diagram

![Sequence diagram](https://user-images.githubusercontent.com/38522985/180879135-66fa0150-8afd-4e61-92eb-986162bb6e0b.svg)

## Eventi/azioni

Events/action

<ul>
Biglietteria:
<li>distribuisci_bigliettiE(Elem1, Elem2, Elem3, Elem4, Elem5, Elem6) 
evento esterno chiamato dall’utente. La Biglietteria salva tutti gli elementi in una lista tramite assert e inizia a svegliare tutti gli agenti mandando un messaggio sveglia con Id e un elemento della lista_biglietto.
<li>
<ul>
<ul>
Cliente:
<li>svegliaE(Id,Biglietto) evento esterno fatto partire dalla biglietteria. Qui il cliente salva le informazioni del biglietto tramite operazione assert e fa l’azione messageOperatoreA una sola volta. <li>
<li>messageOperatoreA in questa azione il cliente prende le info del suo biglietto e manda un messaggio all’agente Spedizione o OperazioniVarie per chiede di poter fare l’operazione. <li>
<li>fai_operazioneE evento esterno che riceve il cliente quando l’agente Spedizione o OperazioniVarie concede di fargli fare l’operazione. Il cliente stamperà un messaggio indicando cosa sta facendo e una volta terminato farà l’azione operazione_terminataA. <li>
<li>operazione_terminataA in questa azione il cliente manda un messaggio all’agente Spedizione o OperazioniVarie per avvisare di aver terminato. A questo punto elimina le informazioni del suo biglietto tramite retract <li>
<ul>
<ul>
Spedizione e OperazioniVarie:
<li>inizializza il suo stato di operatore_libero a true tramite assert<\li>
<li>chiedo_di_fare_operazioneE(Id,Biglietto) riceve la richiesta da parte del cliente. In questo caso salva la prenotazione del cliente con Id, Biglietto e Timestamp <li>
<li>operatore_liberoI se c’è qualcuno in fila in questo caso fa l’azione elabora_filaA <li>
<li>elabora_filaA prende il cliente prenotato e si chiede se c’è un altro cliente prenotato con un numero minore, se così fosse da la precedenza a quest’ultimo altrimenti no. A questo punto concede l’operazione attraverso l’azione concedi_operazioneA(Id) <li>
<li>concedi_operazioneA(Id) manda un messaggio fai_operazione al cliente con l’Id in input <li>
<li>termino_operazioneE evento esterno mandato dal cliente che lo avvisa di aver terminato l’operazione. in questo caso l’agente Spedizione o OperazioniVarie può impostare il proprio stato di operatore_libero da false a true e può ‘dimenticarsi’ del cliente andando ad eliminare le informazioni del cliente_prenotato tramite retract.<li>
<ul>
