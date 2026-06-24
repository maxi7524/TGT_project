Notatki z konsultacji
wzór metylacji jest w stanie się dynamicznie zmieniać
jednej - dwóch generacji
problem w badaniu oddalonych w czasie od siebie pokoleń
trudnije będzie odróżnić to przejście
od szumu metylacji

Wyłuskać sygnał strategii a nie życie w czasach innej temperatury itp.
Uzasadnić podejście do strategii oparta - w jaki sposób umozliwi weryfikację hipotezy.
Ma się dać odpowiedzieć na zajęciach technikami używanymi. Poszukajmy metody wykrywania metylacji inaczej niż mikromacierzami - żeby było połączone z naszymi zajęciami. PAcBio i Nanopore skomercjalizowali sekwencjonowanie dna metylacji która była w nim już kiedyś - poszukać sekwencjonowania jako alternatywa do mikromacierzy, przeprowadzić pipeline bioinformatyczny, nie statystyczny, bo to out of scope tych zajęć. Można spróbować zainspirować się tematem z prezentacji Marcina.
RNA-seq? (tylko czy znajdziemy antyczne RNA)
Czy Deseq działa na DMSach sprawdzić - to byłoby połączenie z zajęciami. Chodzi o pokazanie analizy danych krok po kroku. 
Mail do Karnkowskiej z wyjaśnieniem gdzie używamy terminów z zajęć i spytaniem czy będzie ok jeśli nie chcemy zmieniać tematu.
Można się skonsultować ponownie.
_____________________________________________________________________
24 czerwca
Dodać część wypełniającą potrzebę metodyki z zajęć. 
Sekwencjonowanie czegoś i metody pracy z tym. 
Dodać współczesne na przykład dane.
Oprócz analizy mikromacierzami, genomowa DNA i opisać zanieczyszczenia, może bylibyśmy w stanie te zanieczyszczenia wyeliminować - bakterie i grzyby glebowe, my szukamy ludzkiego, więc dałoby się to rozdzielić - standardowy problem w wielu próbach.
Większym probleme jest degradacja a nie zanieczyszczenia. 
Jest pofragmentowane, więc wziąć ludzkie dane z DNA - kontrolowane, pofragmentować tak jak w antycznym wychodzi i te zanieczyszczenia o których wiemy - prześledzić czy wymyślony pipeline działa. Można w ten sposób parametry też ustawić. I to już pokrywa dużą część metodyki genomowej - kontrola całego procesu.
Autorzy proponują mikromacierze, my robimy high-throughput i określamy, czy jest to realne naszym badaniem. Dane, które sztucznie zanieczyszczamy, żeby wiedzieć jak to wygląda. Dwa wyzwania - pofragmentowanie i zanieczyszczenie. Przy jakich poziomach 
Niskie pokrycie - wyzwanie nr 3. Kolejny parametr do skontrolowania i przetestowania. Pytanie do jakiego wyniku dążymy. Pooling - większa liczba osobników - zostawić. Dalej pipeline roam. Wykrywanie differentially methylated regions. Odpowiednik pakietu deseq, ale trochę więcej. To jakoś przejdzie - mamy mapowanie itp., więc do projektu git, bo analogiczne etapy do składania genomu. Zaplanować, ile danych wygenerujemy. 
Jak znajdziemy metylację, czy technicznie da się wiedzieć, co to za fragment genomu, to zależy od jakości złożenia, u nas się da nowszymi metodami. Adnotacja funkcjonalna. Mapowanie (złożone kontigi) na genom ludzki, identyfikacja genów.
Hunters gatheresrs i agrarne inne geny metylowane, chcemy to porównać z dzisiejszymi - czy dziś mamy takie grupy - są to grupy autochtoniczne ze środkowej afryki, na przestrzeni pokoleń zmienia się u nich metylacja, porównujemy to z grupą kontrolną. 
Jak wychwycić te różnice? Zostało to zasugerowane w artykule, konkretne mutacje - na współczesnej populacji wychwycić regiony i przenieść na antyczne czy to się pokrywa. 
Ocena za to, że umiemy zastosować to, co było na zajęciach. 
Kontrola z danych symulowanych, poddać DNA procesom degradacji.
Mapowanie do eliminacji mismatchy.
Jakie parametry do NGSa na antycznym DNA, czy jest inny sposób robienia biblioteki opisać.
Uwypuklić, co będzie problematyczne - kontaminacja, pokrycie itp.
Można jej to jeszcze wysłać na koniec przed terminem.



