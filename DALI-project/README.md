# DALI-postOffice

Il progetto è volto a gestire la coda negli uffici postali.

## Organizzazione delle cartelle

La cartella DALI-project contiene due sottocartelle application e src. Per poter eseguire il progetto è necessario andare in application e da li far partire il termina con il seguente comando 'bash startmas.sh'.

### Cartella MAS
Nella cartella mas in application/mas abbiamo gli agenti del programma.
In particolare abbiamo gli agenti Cliente che si recano alle poste perchè devono fare una certa operazione (Spedizione/Ritiri oppure Altre Operazioni). Da qui, abbiamo sia l'agente che gestirà le Spedizioni e/O Ritiri che l'agente per le Operazioni Varie. Un cliente arrivato in ufficio postale sarà munito di un biglietto utile per mettersi in fila ed attendere il proprio turno. Il biglietto sarà consegnato dall'agente Biglietteria.



## Esecuzione del programma
Il programma parte da una chiamata esterna da parte dell'utente verso l'agente Biglietteria con dei numeri interi in input che rappresentano i biglietti da consegnare ad ogni agente.
la Biglietteria a questo punto sveglia tutti gli agenti che devono fare una certa operazione assegnando i biglietti per potersi mettere in fila. L'agente Cliente salva il proprio Id e il biglietto assegnato e aspetta il proprio turno. Saranno gli agenti Spedizione e OperazioniVarie a gestire la fila secondo il numero di biglietto di ciascun Cliente. 

All'inizio si esegue il comando send_message(distribuisci_biglietti(e1,e2,e3,e4,e5,e6),user) dall'utente verso agentBiglietteria. 
