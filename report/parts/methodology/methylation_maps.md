
#### Wstęp

Celem tej części metodologii jest odtworzenie populacyjnych map metylacji dla kopalnych kohort walidacyjnych o jednoznacznym trybie życia (pewne HG oraz pewne AGR), a następnie dla populacji niejednoznacznych (np. Çatalhöyük). Mapy te dostarczają wartości $m^j$ wykorzystywanych później w teście klasyfikacyjnym. Ponieważ pojedyncze próbki 1240K mają zbyt niskie pokrycie do rekonstrukcji indywidualnej, mapy budujemy na poziomie populacji, łącząc odczyty wielu osobników z tej samej kohorty. Mapy posłużą potem do walidacji metody.

> Choć Çatalhöyük jest zwykle klasyfikowane jako osiedle rolnicze (AGR), jest przejściowe / niejednoznaczne pod względem strategii bytowania.


#### Procedura

Proces przetwarzania danych obejmuje ekstrakcję sygnału deaminacji, filtrowanie artefaktów, nieliniowe dopasowanie histogramów oraz wygładzanie sygnału metodą okien przesuwnych zgodnie z podejściem zaimplementowanym w pakietach referencyjnych [@barouch2024reconstructing; @mathov2025roam]. Poniżej opisano kolejne kroki potoku — od doboru kohort, przez przygotowanie danych sekwencyjnych i właściwą rekonstrukcję metylacji, po detekcję regionów różnicowo zmetylowanych — zachowując analogiczną strukturę względem części dotyczącej populacji współczesnych.

##### Pozyskanie i selekcja kohort kopalnych

1. **Dobór kohort i tkanki**
   - **Tkanka**: preferowana kość skalista (*petrous bone*) — minimalizuje zmienność wewnątrzpopulacyjną wynikającą z tkankowej specyficzności metylacji oraz zapewnia najwyższy udział endogennego aDNA [@barouch2024reconstructing].
   - **Typ danych**: pozyskanie odczytów przebiega dwuetapowo. Najpierw biblioteka aDNA jest **wzbogacana hybrydyzacyjnie** *in-solution* z panelem sond **1240K** (~1,24 mln pozycji SNP) — etap ten pełni rolę selekcji/filtra, ograniczając materiał do z góry zdefiniowanego zestawu pozycji genomowych. Następnie tak uzyskaną bibliotekę poddaje się **sekwencjonowaniu nowej generacji (NGS)** — w praktyce sekwencjonowaniu krótkich odczytów na platformie Illumina. Ze względu na poziom degradacji aDNA spodziewamy się niskiego pokrycia. Ponieważ sondy obejmują region ~100 pz wokół każdej pozycji docelowej, odczyty pokrywają również sąsiadujące pozycje CpG, co umożliwia rekonstrukcję metylacji mimo braku **laboratoryjnego protokołu pomiaru metylacji** (np. konwersji dwusiarczynowej / WGBS lub macierzy metylacyjnych, które mierzą metylację bezpośrednio). Zamiast tego metylację odtwarza się *post hoc*, obliczeniowo (*in silico*), z sygnału deaminacji za pomocą narzędzi takich jak RoAM czy epiPALEOMIX. Jest to najczęściej stosowana technologia w paleogenomice, lecz pokrycie pojedynczej próbki pozostaje zbyt niskie do rekonstrukcji indywidualnej [@barouch2024reconstructing].
   - **Homogeniczność**: do jednej kohorty łączy się osobników z tego samego okresu i regionu geograficznego, aby mapa odzwierciedlała charakterystyczny profil populacyjny.
   - **Liczebność**: dążyć do $\ge 20$ osobników na kohortę (rząd wielkości jak w pracy referencyjnej, gdzie kohorty liczyły 21–142 osobników) [@barouch2024reconstructing].

2. **Filtrowanie osobników**
   - Wykluczenie próbek o liczbie trafień w autosomalne SNP $< 30\,000$ — niższe pokrycie obniża wiarygodność rekonstrukcji [@barouch2024reconstructing].

3. **Metadane**
   - Okres, region, tryb życia (HG / AGR / niejednoznaczny) — wykorzystywane jako etykiety populacyjne w teście klasyfikacyjnym oraz przy detekcji DMR.

##### Wstępne przetwarzanie odczytów i mapowanie do genomu referencyjnego

Surowe odczyty kopalnych bibliotek poddaje się kontroli jakości i przycinaniu adapterów (`FastQC`, `fastp`), a następnie mapuje do ludzkiego genomu referencyjnego (hg19). Należy uwzględnić, że odczyty aDNA są **bardzo krótkie** (zwykle ~30–70 pz wskutek pośmiertnej fragmentacji), co utrudnia jednoznaczne mapowanie — krótkie odczyty częściej dopasowują się wieloznacznie, szczególnie w regionach powtarzalnych. Z tego powodu w analizie aDNA standardem jest program mapujący `bwa aln`/`samse` (BWA-backtrack), który lepiej radzi sobie z krótkimi i uszkodzonymi odczytami niż `bwa mem` (zaprojektowany dla odczytów > ~70 pz); dodatkowo odrzuca się odczyty krótsze niż ~30–35 pz, stosuje filtr jakości mapowania (MAPQ) oraz rozluźnia parametry dopasowania lub przeskalowuje oceny jakości poszczególnych zasad (np. programem `mapDamage`), aby uwzględnić niedopasowania wynikające z deaminacji na końcach fragmentów. Zastosowanie sond 1240K częściowo łagodzi ten problem, ponieważ sondy celują w unikatowe, dobrze zdefiniowane loci wokół SNP (~100 pz), do których nawet krótkie odczyty mapują się względnie jednoznacznie. Powstałe pliki BAM sortuje się, indeksuje oraz oczyszcza z duplikatów PCR przy użyciu `samtools`, a wynik można skontrolować wizualnie w przeglądarce **IGV**. Stanowią one wejście do właściwej rekonstrukcji metylacji: pakiety referencyjne (RoAM, epiPALEOMIX) przyjmują na wejściu pliki BAM pojedynczych osobników, przetwarzane chromosom po chromosomie [@mathov2025roam; @hanghoj2016fast].

##### Ekstrakcja sygnału metylacji (wskaźnik C → T)

Sygnał metylacji odzyskuje się ze wzorca deaminacji — dominującego procesu degradacji aDNA [@briggs2010removal]. Niemetylowana cytozyna deaminuje do uracylu (usuwanego przez traktowanie **USER/UDG**), natomiast metylowana 5-metylocytozyna deaminuje do tyminy. Dla pozycji $i$ definiuje się wskaźnik

$$\text{C} \rightarrow \text{T}\,(i) = \frac{t_i}{t_i + c_i},$$

gdzie $t_i$ oraz $c_i$ to liczby odczytów z tyminą i cytozyną w danej pozycji CpG; wyższy wskaźnik odpowiada wyższemu poziomowi metylacji przedśmiertnej [@gokhman2014reconstructing; @barouch2024reconstructing]. Dla bibliotek dwuniciowych zlicza się dodatkowo komplementarne zdarzenia G → A na nici przeciwnej [@mathov2025roam].

##### Filtrowanie pozycji CpG

Z analizy usuwa się pozycje niewiarygodne dla rekonstrukcji [@barouch2024reconstructing; @mathov2025roam]:
- **Duplikaty PCR** — pozycje o nadmiernym pokryciu, identyfikowane na podstawie histogramu pokrycia liczonego per chromosom (próg wyznaczany automatycznie, np. metodą rozstępu międzykwartylowego z parametrem rozpiętości w RoAM).
- **Domniemane mutacje C → T** — pozycje o wskaźniku C → T przekraczającym próg (np. $> 0{,}25$); w RoAM dodatkowo modeluje się rozkład tymin jako mieszaninę trzech rozkładów dwumianowych (mutacje homozygotyczne, heterozygotyczne, deaminacja) i wyznacza próg metodą EM.
- Po filtrowaniu łączy się liczniki C i T z obu nici tej samej pozycji CpG, zwiększając ilość informacji.

##### Estymacja tempa deaminacji

Tempo deaminacji $\pi$ estymuje się na podstawie pozycji CpG, których metylacja w referencyjnej mapie współczesnej kości jest bliska jedności: $\hat{\pi} = \sum t_i / \sum n_i$ po tych pozycjach. Przy braku referencji można skorzystać z założonej globalnej średniej metylacji w próbce [@mathov2025roam].

##### Pulowanie osobników i rekonstrukcja mapy populacyjnej

Ponieważ pojedyncze próbki 1240K mają zbyt niskie pokrycie, mapę buduje się na poziomie populacji przez **Łączenie** odczytów wielu osobników [@barouch2024reconstructing]:
- **Łączenie naiwne** — proste sumowanie liczników C i T w każdej pozycji CpG ze wszystkich osobników; w praktyce działa równie dobrze lub lepiej niż wariant „zaawansowany” uwzględniający indywidualne tempa deaminacji (estymacja największej wiarogodności rozwiązywana metodą Newtona–Raphsona).
- **Mapowanie C → T na metylację** metodą *histogram matching* — nieliniowe dopasowanie histogramu wskaźników C → T do referencyjnego histogramu metylacji we współczesnej kości; RoAM oferuje alternatywnie transformację liniową obciętą oraz logistyczną [@mathov2025roam].
- **Wygładzanie w przesuwnym oknie** — rekonstrukcję prowadzi się w oknie $W$ kolejnych pozycji CpG (nieparzyste, wartość przypisana środkowej pozycji), gdyż poziomy metylacji sąsiadujących CpG są silnie skorelowane (~2 kpz). Rozmiar okna dobiera się automatycznie (metoda probabilistyczna lub oparta na błędzie względnym), zależnie od efektywnego pokrycia kohorty.

##### Kontrola efektywnego pokrycia

Wiarygodna rekonstrukcja wymaga **efektywnego pokrycia $\ge 12$**. Efektywne pokrycie kohorty można oszacować z góry prostym modelem regresji liniowej z liczby pulowanych osobników i ich średniej liczby trafień w autosomalne SNP ($R^2 = 0{,}99$ w pracy referencyjnej) [@barouch2024reconstructing].

##### Detekcja regionów różnicowo zmetylowanych (DMR) i kontrola FDR

Mapy dwóch populacji porównuje się w celu wykrycia DMR. Odsetek fałszywych odkryć (FDR) kontroluje się przez **permutacje etykiet populacyjnych**: DMR znalezione w danych z przetasowanymi etykietami traktuje się jako fałszywe odkrycia, a FDR liczy się jako stosunek średniej liczby DMR w permutacjach do liczby zaobserwowanej. Parametry wykrywania (m.in. próg różnicy metylacji oraz minimalna liczba CpG w regionie) dobiera się tak, aby uzyskać $\text{FDR} < 0{,}05$ [@barouch2024reconstructing; @mathov2025roam].

##### Walidacja biologiczna map

Poprawność zrekonstruowanych map weryfikuje się sprawdzeniem oczekiwanych cech biologicznych: hipometylacji wysp CpG i promotorów genów *housekeeping*, podwyższonej metylacji elementów powtarzalnych oraz najwyższej korelacji z metylomem komórek kości (zgodnie ze szkieletowym pochodzeniem aDNA) [@barouch2024reconstructing]. Dopiero zwalidowane mapy populacji o jednoznacznym trybie życia (pewne HG, pewne AGR) służą jako odniesienie przy klasyfikacji populacji niejednoznacznych (np. Çatalhöyük).


