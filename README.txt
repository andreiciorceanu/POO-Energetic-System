324CA Ciorceanu Andrei-Razvan

Sumar : 
Tema presupune simularea unui ecosistem de producatori/consumatori de energie electrica.
Input :
Clasa SimulationInputData contine toate detaliile de input:
	- numberOfTurns 
	- InitialData - listele initiale cu consumatori si distribuitori 
	- MonthlyUpdates - listele de consumatori noi si CostChange
	- CostChange- schimbarile pentru distribuitori

Clase: 
Consumer, Producer ce extind Entity - modeleaza notiunile de entitate in piata respectiv consumatori/consumatori
Entity - un identificator, buget si un flag de faliment 
Consumer - detine functii ce modifica bugetul ( daca poate plati un contract, daca e falimentar)
Distributor - detine functii ce modifica bugetul ( producerea de oferte, daca poate plati un contract, daca e falimentar )

Offer - modeleaza o oferta data de un distribuitor pe piata cu un pret si o perioada
Contract ce extinde Offer - provide din detaliile unei oferte si leaga oferta de consumatori
	- are stari interne in functie de actiunile contractului :
		- ACTIVE - un contract eligibil pentru a fi platit de catre consumatori catre producatori/consumatori
		- POSTPONED - un contract amanat etapa in care nu are un cost pentru consumatori si nici un profit pentru distribuitori
		- EXPIRED - un contract ce a ajuns la final ce urmeaza sa fie eliminat 

ContractService - SINGLETON -  manager pentru oferte si contracte
	- detine ofertele si adauga/modifica/sterge ofertele primite de la distribuitori
	- detine contractele adauga/modifica/sterge contracte cu consumatorii
	- detine detalii despre contracte in momentul expirarii

EntityFactory - factory method pattern pentru producerea de entitati	


Driverul procesarii este reprezentat de clasa Simulation, care efectueaza toate etapele simularii:
Simulation : SINGLETON
	- setup  - importa starea initiala a consumatorilor/producatorilor
	- apply  - adauga noi consumatori
			 - face update la detaliile producatorilor
	- offer  - pentru toti distribuitorii produce cate o oferta(Offer class) 
	- assign - pentru toti consumatorii produce un contract(Contract class) care leaga consumatorii de distribuitori
	- update - executa schimbarile de buget pentru consumatori si face modificarile de contract in cazul unui faliment
			 - muta contractele in luna urmatoare 
			 - executa schimbarile de buget pentru distribuitori si face modificarile de contracte in cazul unui faliment
Simulation va executa aceste stagii la fiecare turn si va contine starea finala dupa ultimul turn. 
Folosing jackson, scriu aceasta clasa in fisierul de iesire la finalul simularii.
