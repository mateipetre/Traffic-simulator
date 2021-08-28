# Traffic-simulator

Homework from 'Parallel and Distributed Algorithms' course


                                          Tema #2 - APD
                        		            Traffic simulator


	In implementarea temei am folosit clasele din scheletul oferit si majoritatea logicii
	se gaseste in clasa IntersectionHandlerFactory, dar am folosit pentru citirea informatiilor
	suplimentare si clasa ReaderHandlerFactory, iar pentru declarari si initializari ale unor
	variabile ajutatoare m-am folosit de Main.

				** Cerinta 1 **: ** simple_semaphore **
	
	La acest task am afisat din metoda handle din IntersectionHandlerFactory faptul ca masina a 
	ajuns la semafor, am pus un sleep intr-un bloc try-catch pentru ca masina sa astepte timpul
	necesar la semafor. La sfarsit am afisat faptul ca masina trece de semafor dupa ce a
	asteptat timpul necesar astribuit citit din fisier.

				** Cerinta 2 **: ** simple_n_roundabout **

	La acest task am afisat din metoda handle din IntersectionHandlerFactory faptul ca masina 
	a ajuns in sensul giratoriu. In clasa principala Main am creat un semafor initializat cu
	'n' permit-uri care lasa masina sa intre in sens doar daca semaforul are numarul de permit-uri
	diferit de 0 prin apelarea metodei acquire pe el. Acest numar se decrementeaza cu 1. 
	Daca masina trece de semafor afisez faptul ca masina a intrat in sens.
	Masina asteapta timpul necesar prin apelul unui sleep, dupa care afisez faptul 
	ca iese din sens. Dupa ce iese din sens, se poate da release pe semafor,
	deci numarul de permit-uri se incrementeaza cu 1.
	

				** Cerinta 3 **: ** simple_strict_1_car_roundabout **

	La acest task m-am folosit de un ConcurrentHashMap 'lanes' care va tine evidenta lane-urilor
	, deci retine cate masini mai sunt pe fiecare lane la un anumit moment de timp. Initial
	pe fiecare lane se afla acelasi numar de masini 'numberOfCarsOnLane'. Am distribuit fiecarui
	lane acest numar de masini initial, deci fiecarei chei a hashmap-ului (reprezentat de
	lane) i-am atribuit aceasta valoare. In metoda handle am afisat faptul ca masina ajunge
	la sensul giratoriu. Am pus toata logica urmatoare intr-un bloc synchronized pe obiectul
	'lanes'. O masina de pe un anumit lane nu va putea intra in sensul giratoriu daca sunt
	mai multe masini pe celelalte lane-uri la un anumit moment de timp si va astepta pana cand
	celelalte lane-uri nu vor mai avea un numar mai mare de masini care vor sa intre in sens,
	intrucat poate sa intre doar 1 de pe fiecare lane. Daca trece de lock-ul pus pe 'lanes',
	masina intra in sens si se retine acest lucru in hashmap-ul de lane-uri prin decrementarea
	valorii de la cheia respectiva (lane-ul de pe care a intrat masina). Masina va astepta
	timpul necesar de iesire din sens printr-un sleep dupa care le notifica si pe celelalte masini
	care asteapta de pe alte lane-uri ca a intrat in sens. La sfarsit masina va iesi din sens.

				** Cerinta 6 **: ** priority_intersection **

	La acest task m-am folosit de un vector de vectori. In vectorul mare creez 2 subvectori
	, unde un subvector va contine id-urile masinilor cu prioritate (subvectorul 0) si celalalt
	va contine id-urile masinilor fara prioritate (subvectorul 1). Intr-un bloc synchronized
	pe vectorul mare care contine acesti 2 subvectori, am implementat toata logica din handle.
	Adaug masinile fara prioritate in subvectorul corespunzator dupa id-uri si ve afisa
	faptul ca masina incearca sa treaca de intersectie. Daca exista masini cu prioritate deja
	in intersectie, adica exista id-uri in subvectorul cu masinile cu prioritate, atunci masina
	fara prioritate va astepta sa iasa acestea mai intai. Adaug si masina cu prioritate in 
	celalalt subvector si aceasta intra direct in intersectie, asteapta cele 2 secunde pana iese
	, apoi iese din intersectie si ii dau remove si din subvector. Dupa ce iese, aceasta va notifica
	toate masinile ca a iesit din intersectie. Masina fara prioritate va iesi din vector daca
	nu sta in asteptare din cauza wait-ului si a primit notificare de la una cu prioritate si 
	va intra si iesi instant din intersectie.	

				** Cerinta 7 **: ** crosswalk **

	La acest task tin evidenta din handle a mesajelor afisate prin stocarea acestora in niste
	variabile declarate initial. Logica este urmatoarea: cat timp trec pietoni in intervalul dat
	de timp (finished nu e 1), masinile vor merge in cerc. Daca masinile se afla la trecere si
	trec pietoni in acel moment culoarea semaforului se va face rosu, altfel se va face verde. 
	Retin mesajul corespunzator si daca e diferit de cel afisat anterior, se va afisa, altfel nu.
	Daca finished e 0, se verifica din nou daca mai trec sau nu pietoni si se asigura afisarea
	unui mesaj corespunzator.

				** Cerinta 10 **: ** railroad **

	La acest task, m-am folosit tot de un vector de vectori asemanator cu cel de la task-ul precedent,
	si e asemanator cu o matrice cu 2 linii care reprezinta sensurile de mers. Pe aceste 2 linii
	(vectori) se vor retine id-urile masinilor in ordinea in care au ajuns masinile. Am creat 
	in Main cele 2 "sensuri" 0 si 1, initial goale. Sincronizez totul pe clasa Car. Masina ajunge
	la calea ferata si se va afisa mesajul corespunzator din metoda handle (bariera e lasata in jos).
	Se va incadra masina pe sensul corespunzator adaugand-o intr-unul din cei 2 subvectori, vazuti
	ca 2 linii dintr-o matrice. Se pune o bariera pentru ca toate masinile sa astepte toate
	pana ce trece trenul, afisare ce va fi facuta de o singura masina (eu am ales sa faca asta
	thread-ul care are id-ul ultimei masini considerate). Toate masinile asteapta si pana se ridica 
	bariera (bariera caii ferate) si apoi trec in ordinea in care au ajuns la ea in felul urmator:
	va trece de calea ferata prima masina care a ramas in rand pe fiecare sens in functie de sens. 
	Ea va fi scoase de pe sensul pe care s-a aflat initial prin remove-ul din subvectorul 
	corespunzator al primului element. Astfel masina cu id-ul respectiv va trece de calea ferata si
 	se va afisa un mesaj corespunzator.

	OBS: La task-ul 2 am folosit clasa Semaphore pentru sincronizare, la task-ul 3 un mecanism
	de tipul wait-notifyAll, la task-ul 6 tot un mecanism wait-notifyAll, la task-ul 10
	bariera de tip CyclicBarrier. In general am folosit blocuri synchronized.
