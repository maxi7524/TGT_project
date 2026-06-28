
### Aktualny stan wiedzy

#### Metody rekonstrukcji map metylacji

##### Wykorzystane zjawisko - deaminacja cytozyn

Rekonstrukcja map metylacji z kopalnego DNA (aDNA) opiera się na zjawisku deaminacji cytozyn – dominującym procesie chemicznego rozkładu aDNA [@briggs2010removal]. Niemetylowana cytozyna deaminuje do uracylu ($C \to U$), natomiast metylowana 5-metylocytozyna deaminuje do tyminy ($5\mathrm{mC} \to T$). Tempo deaminacji jest silnie zależne od struktury fragmentu: w jednoniciowych cząsteczkach na końcach fragmentów sięga kilkudziesięciu procent, podczas gdy w częściach dwuniciowych wynosi około 1% lub mniej; uśredniony wskaźnik deaminacji fragmentu mieści się zwykle w przedziale 1–3%. Protokoły przygotowania bibliotek z (częściowym lub pełnym) traktowaniem USER / UDG (*Uracil-Specific Excision Reagent*) usuwają uracyl, ale nie naruszają tymin. Dzięki temu udział tymin na wyspach CpG (gdzie cytozyna jest metylowana w genomie ssaków), zwany wskaźnikiem $C \to T$, może służyć jako estymator przedśmiertnego poziomu metylacji.

Dla pozycji $i$ definiuje się wskaźnik:

$$
C \to \mathbf{T}(i) = \frac{t_i}{t_i + c_i}
$$

Gdzie $t_i$ oraz $c_i$ to liczby odczytów z tyminą i cytozyną na danej pozycji. Wyższy wskaźnik odpowiada wyższemu poziomowi metylacji. Ponieważ poziomy metylacji sąsiadujących CpG są silnie skorelowane (ko-metylacja w obrębie regionów rzędu $\approx 2$ kpz), estymację wygładza się w przesuwnym oknie obejmującym kilka–kilkadziesiąt kolejnych CpG, co radykalnie redukuje błąd standardowy estymatora.

***

##### Metoda oryginalna 

Metoda oryginalna [@gokhman2014reconstructing] dotyczyła pierwszej pełnej rekonstrukcji metylomu neandertalczyka i denisowianina. Wymaga próbek o wysokim pokryciu shotgun ($\ge \approx 15\times$) poddanych traktowaniu USER. Porównanie z metylomami współczesnych ludzi pozwoliło zidentyfikować $\approx 2000$ regionów różnicowo zmetylowanych (DMR), w tym istotne zmiany w klastrze *HOXD*, potencjalnie tłumaczące różnice anatomiczne między człowiekiem archaicznym a współczesnym. Ograniczeniem jest jednak dostępność danych: bardzo niewiele próbek aDNA spełnia jednocześnie oba wymagania — wystarczająco wysokie pokrycie oraz przygotowanie biblioteki z traktowaniem USER. W praktyce metodę zastosowano dotąd jedynie do nielicznych okazów (neandertalczyk, denisowianin oraz kilku anatomicznie współczesnych ludzi).
***

##### Adaptacja metody do danych o niskim pokryciu (Barouch i in., 2024, NAR)

Zdecydowana większość kopalnych osobników jest sekwencjonowana metodą hybrydyzacyjnego wzbogacania (*in-solution capture*), najczęściej z użyciem panelu 1240K ($\approx 1,24$ mln SNP) – sekwencjonowanie tylko niewielkich fragmentów genomu, co daje dane zbyt płytkie do rekonstrukcji metylacji na poziomie pojedynczego osobnika. Autorzy [@barouch2024reconstructing] wykorzystują obserwację, że zmienność metylacji wewnątrz populacji jest mniejsza niż między populacjami – populacje mają więc charakterystyczne profile metylacyjne. Zamiast rekonstruować mapę indywidualną, łączą odczyty wielu osobników z tej samej populacji, uzyskując mapę metylacji reprezentatywną dla całej populacji. Kluczowe elementy procedury:

- **Naiwne łączenie (naive pulling)** – proste sumowanie liczników $C$ i $T$ w każdej pozycji CpG ze wszystkich osobników; działa równie dobrze lub lepiej niż wariant „zaawansowany” uwzględniający indywidualne tempa deaminacji (osobnicy z tego samego miejsca i okresu przeszli zbliżone procesy deaminacji).
- **Filtrowanie** – usunięcie domniemanych duplikatów PCR oraz pozycji CpG o wskaźniku $C \to T > 0,25$ (prawdopodobnie mutacje, a nie deaminacja).
- **Mapowanie $C \to T$ na metylację** metodą *histogram matching* – nieliniowe dopasowanie histogramu wskaźników $C \to T$ do referencyjnego histogramu metylacji we współczesnej kości człowieka (WGBS).
- **Adaptacyjny rozmiar okna** – do 31 kolejnych CpG, dobierany w zależności od efektywnego pokrycia kohorty.
- **Próg jakości** – do wiarygodnej rekonstrukcji potrzebne jest efektywne pokrycie $\ge 12$ (najlepiej $> 15$). Efektywne pokrycie kohorty można z góry oszacować modelem liniowym z liczby osobników i średniej liczby trafień w autosomalne SNP (*single-nucleotide polymorphism*).

> Panel 1240K nie jest połączenie dwóch wcześniejszych paneli wzbogacania opracowanych w laboratorium Davida Reicha (Harvard) we współpracy z grupą Nicka Pattersona i innymi. Stąd bywa nazywany „Reich panel”. Jest używany, ponieważ w kopalnych kościach/zębach DNA endogenne (ludzkie) to często $<1\%$ całego materiału, pozostała część to bakterie glebowe, grzyby, zanieczyszczenia.

Walidacja wykazała, że zrekonstruowane mapy populacyjne odtwarzają oczekiwane cechy biologiczne: hipometylację wysp CpG i promotorów genów *housekeeping*, wysoką metylację elementów Alu, a także najsilniejszą korelację z metylomem osteoblastów (zgodnie ze szkieletowym pochodzeniem aDNA). Detekcję DMR między populacjami przeprowadza się z kontrolą odsetka fałszywych odkryć (FDR – *false discovery rate*) poprzez permutacje etykiet populacyjnych.

***

##### Narzędzia obliczeniowe

Podejście łączenia [@barouch2024reconstructing] jest dla nas kluczowe: umożliwia uzyskanie populacyjnych map metylacji z tysięcy już dostępnych, niskopokryciowych próbek 1240K – w tym z populacji bezpośrednio związanych z przejściem HG $\to$ AGR (np. brytyjski neolit i epoka brązu, kultura pucharów dzwonowatych). Pozwala to prowadzić porównania HG vs. AGR na poziomie populacji, mimo że pojedyncze próbki są zbyt płytkie do klasycznej rekonstrukcji metylomu. Do zautomatyzowanego przetwarzania wykorzystuje się potoki takie jak epiPALEOMIX [@hanghoj2016fast] oraz RoAM [@mathov2025roam].

##### Cechy epigenetyczne w ludnościach autochtonicznych


#TODO przepisać i przenieść bibliografie 
Aktualnie wykazano, że istnieje istotna różnica pomiędzy metylacją w populacjach HG i AGR oraz scharakteryzowano, że jest to związane z regulacją immunologiczną oraz rozwojem [@fagny2015epigenomic]. Badania jednak były wykonane na komórkach krwi.

##### Analizy wykorzystujące metylacje

Aktualnie wykorzystano metylację do scharakteryzowania różnic w mapach metylacyjnych u Neandertalczyków oraz Denisovian [@gokhman2014reconstructing] oraz zidentyfikowano ERLs związane z głodem i stwierdzono, że w populacjach HG występowały trendy zmian o charakterze adaptacyjnym [@inferring2017past]. Nie zidentyfikowano jednak charakterystycznych czynników epigenetycznych różnicujących grupy HG i AGR na poziomie tkanki kostnej.
