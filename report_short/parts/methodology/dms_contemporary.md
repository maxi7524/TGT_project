
#### Wstęp

Dotychczasowa charakterystyka różnic epigenetycznych między populacjami ($\text{HG}$) a ($\text{AGR}$) opiera się przede wszystkim na analizie jednej pary populacji z jednego regionu: afrykańskich łowców-zbieraczy lasu deszczowego oraz sąsiadujących z nimi rolników [@fagny2015epigenomic].

Jednakowoż, mała liczba niezależnych populacji ogranicza moc statystyczną i wiarygodność identyfikowanych miejsc różnicowo metylowanych ($\text{DMS}$), a różnice obserwowane między jedną parą $\text{HG}/\text{AGR}$ mogą być zaburzane przez pochodzenie genetyczne, lokalną historię demograficzną oraz specyfikę środowiska, w tym klimat lasu równikowego [@fagny2015epigenomic].

Rozwiązaniem tego problemu jest włączenie wielu niezależnych par $\text{HG}/\text{AGR}$ pochodzących z różnych kontynentów i stref klimatycznych. 

#### Procedura

##### Identyfikacja miejsc różnicowo zmetylowanych (DMS)

Dane EM-seq (liczba odczytów metylowanych i niemetylowanych w każdej pozycji CpG) modelujemy testem opartym na rozkładzie beta-dwumianowym, uwzględniającym zmienność biologiczną i pokrycie (pakiet `DSS`) [@feng2014dss], porównując średni poziom metylacji między dwiema populacjami (HG vs. AGR) testem Walda. Zmienne zakłócające: płeć, wiek, proporcje typów komórek, w analizie referencyjnej wprowadzane jako współzmienne [@fagny2015epigenomic], ograniczamy przed testem przez dopasowanie i stratyfikację próbek. Wartości P koryguje się metodą Benjamini-Hochberga; za DMS uznaje się pozycje o $P < 0{,}01$ oraz różnicy poziomu metylacji ($\Delta\beta$) ponad 10% [@fagny2015epigenomic]. Istotność nakładania się zestawów DMS ocenia się ponownym próbkowaniem, uwzględniając korelację w regionach ~2000 bp [@fagny2015epigenomic].

##### Analiza funkcjonalna i ontologia genów

Geny zawierające co najmniej jeden DMS poddaje się analizie nadreprezentacji kategorii ontologii genów (GO) pakietem `goseq` [@young2010goseq; @fagny2015epigenomic]; jako miarę obciążenia detekcji stosujemy liczbę pozycji CpG o wystarczającym pokryciu na gen. Kategorię GO uznaje się za istotnie wzbogaconą dla wartości P ≤ 0,05 [@fagny2015epigenomic].
