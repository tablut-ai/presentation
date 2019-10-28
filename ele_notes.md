NegaScout (da vedere) 
Alpha-beta pruning con killer heuristic e transposition table 

Documentazione:  
Alpha-beta pruning,  
Alpha-beta pruning video,  
Transposition Table 
Killer Heuristic 

Elementi centrali da sviluppare 
-	Rappresentazione dello stato del gioco (pedine, tavola da gioco, regole,  input ecc) 
-	Relaxed problems (possibili funzioni euristiche) 
-	Transposition Table e Killer Heuristic 
-	Pro e contro iterative deepening 
-	Timeout mossa 
-	Comunicazioni con il server (Dani, ci serve la tua sapienza) 
 
1) Rappresentazione dello stato del gioco 

-	Descrizione gioco 

2 giocatori: 
-	Difensore (bianco), muove per primo
-	Attaccante (nero)

Pedine:   
-	Nero: 16 pedine cavaliere 
-	Bianco: 8 pedine cavaliere, 1 pedina Re 

Vittoria: 
-	Bianco: far fuggire il Re (vedi Caselle Fuga) 
-	Nero: catturare il Re (vedi Cattura) 



Tavola da gioco: griglia 9x9 con 4 tipi di caselle 
-	Casella Castello:  
-	posizione iniziale del Re 
-	Casella proibita per tutte le pedine Cavaliere 
-	Casella proibita per il Re, una volta mosso 
-	Caselle Accampamento:  
-	posizione iniziale dei cavalieri Neri;  
-	sono caselle proibite a tutte le pedine bianche e alle pedine nere uscite dall’accampamento. 
-	Caselle Normali 
-	Caselle Fuga:  
-	posizione di vittoria del Re 
-	sono viste come caselle Normali dalle altre pedine 
-	Caselle Proibite:  
-	non è possibile attraversarle  
-	Non è possibile terminare un movimento su di esse 
-	Tutte le caselle occupate da una qualsiasi pedina sono proibite 








Mosse 
Ogni pedina può muoversi di quante caselle vuole in direzione ortogonale.
Non è possibile attraversare o terminare su una casella proibita. 








Cattura 
La cattura è attiva e avviene circondando una pedina avversaria su due lati opposti. 
È possibile catturare più pedine contemporaneamente. 
Di seguito sono elencati alcuni casi particolari: 

-	Se il Re è nel Castello deve essere circondato su tutti e 4 i lati. 

-	Se il Re è adiacente al Castello, deve essere circondato sui 3 lati liberi. 

-	Il Castello funge da sponda per le pedine Cavaliere: se un Cavaliere è adiacente al castello è sufficiente circondarlo con una pedina sul lato opposto al Castello. Non importa se nel castello c’è il Re o meno

-	Le caselle Accampamento fungono da sponda: se una qualsiasi pedina è adiacente a un Accampamento, è sufficiente circondarla con una pedina sul lato opposto all’Accampamento. Non importa se la casella dell’accampamento è occupata o meno. 

Termine partita
-	Vittoria del bianco
-	Vittoria del nero
-	Un giocatore non può muovere nessuna pedina in nessuna direzione: sconfitta di quel giocatore
-	Si raggiunge uno stato già raggiunto in precedenza: pareggio
-	Timeout: perde il giocatore di turno

Tempi di gioco:
-	1 minuto per mossa
-	Timeout deciso dal prof

-	Input e output
Dani aiutami tu, cosa ci arriva dal server per definire lo stato corrente della partita?
In output la mossa scelta, come va inviata? Sempre Dani



-	Possibili funzioni euristiche
La scelta della funzione euristica (anche più di una se necessario) gioca un ruolo centrale nella costruzione della strategia di ricerca, al pari con la scelta dell’algoritmo da implementare.
2.0 Riflessioni libere (utili o inutili lo decideremo)
N.B: tutto ciò che segue è da intendersi inserito in un gigantesco “secondo me”
-	La distanza spaziale intesa come numero di caselle non è informativa, in quanto le pedine possono muoversi di un numero qualsiasi di caselle in una delle 4 direzioni concesse (posto che non vi siano caselle proibite da attraversare). Pertanto, la scelta della funzione euristica non può basarsi sul calcolo di una qualsivoglia distanza spaziale.
-	La scacchiera è simmetrica, per ogni configurazione ne ho altre 3 esattamente identiche: pruning, l’euristica può già fare un lavoro di sfoltimento? O forse si può fare a livello di input?
-	Giocando un po’ ho notato che ci sono alcune posizioni per il re che portano direttamente alla vittoria.
-	Quando giochiamo come bianchi, potrebbe essere computazionalmente comodo avere un set di mossa di apertura  da cui pescare, senza bisogno di valutarle nel search tree.
-	La soluzione potrebbe essere una funzione definita come combinazione lineare di più funzioni che tengano presenti aspetti diversi (uno di questi è senz’altro il numero di pedine in possesso e poi boh, dobbiamo pensarci). I coefficienti della combinazione lineare potrebbero variare durante l’avanzare del gioco (secondo condizioni da definire - sì, lo so, sto scrivendo molte cose generali e poco succo, ma perdonatemi, sono prime idee). In questo modo potremmo sia accelerare la computazione, escludendo uno o più elementi della combinazione, sia adattare l’euristica in un modo abbastanza flessibile.
