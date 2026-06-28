
#### Wstęp

Dotychczasowa charakterystyka różnic epigenetycznych między populacjami łowiecko-zbierackimi ($\text{HG}$) a rolniczymi ($\text{AGR}$) opiera się przede wszystkim na analizie jednej pary populacji z jednego regionu — afrykańskich łowców-zbieraczy lasu deszczowego oraz sąsiadujących z nimi rolników [@fagny2015epigenomic]. Oparcie referencyjnych sygnatur metylacji na pojedynczej parze populacji wiąże się z dwoma istotnymi ograniczeniami.

Po pierwsze, mała liczba niezależnych populacji ogranicza moc statystyczną i wiarygodność identyfikowanych miejsc różnicowo metylowanych ($\text{DMS}$). Po drugie, różnice obserwowane między jedną parą $\text{HG}/\text{AGR}$ mogą być zaburzane przez pochodzenie genetyczne, lokalną historię demograficzną oraz specyfikę środowiska, w tym klimat lasu równikowego [@fagny2015epigenomic]. W takim układzie trudno rozstrzygnąć, czy wykryty sygnał odzwierciedla rzeczywisty efekt trybu życia, czy raczej lokalną różnicę typu „populacja A vs. populacja B”.

Rozwiązaniem tego problemu jest włączenie wielu niezależnych par $\text{HG}/\text{AGR}$ pochodzących z różnych kontynentów i stref klimatycznych. Sygnatury epigenetyczne powtarzalne w wielu takich porównaniach z dużo większym prawdopodobieństwem odzwierciedlają uniwersalny komponent związany z trybem życia, a nie lokalne uwarunkowania genetyczne, demograficzne lub środowiskowe.


#### Procedura

##### Identyfikacja miejsc różnicowo zmetylowanych (DMS)
Określenie miejsc o zróżnicowanym stopniu metylacji między populacjami zostaje zmodyfikowane statystycznie poprzez dopasowanie modelu regresji liniowej dla każdego loci CpG [@fagny2015epigenomic]. Modelowanie opiera się na analizie ciągłych wartości M według równania:

$$\text{M-value} \sim \text{populacja} + \text{płeć} + \text{wiek} + \text{proporcje typów komórek} + \text{błąd losowy}$$

W procesie tym stosujemy wygładzenie empiryczne Bayesa do odchylenia standardowego przy użyciu pakietu `limma` z repozytorium R Bioconductor [@fagny2015epigenomic], następnie wprowadzając korekcje Benjamini-Hochberga. Miejsca charakteryzujące się skorygowaną wartością $P < 0{,}01$ uznaje się za różnicowo metylowane [@fagny2015epigenomic].

Aby zdefiniować amplitudę zmian i wyodrębnić DMS, stosujemy rygorystyczne kryteria obejmujące skorygowaną wartość $P < 0{,}01$ oraz różnicę w średnim poziomie metylacji między dwiema populacjami ($\Delta\beta$) wynoszącą ponad 2, 5 lub 10% [@fagny2015epigenomic]. Poziom metylacji w tej analizie określa się jako stosunek intensywności sygnału z sondy metylowanej do intensywności ogólnej, reprezentujący wartość beta ($\beta$) [@fagny2015epigenomic]. W procesie tym wyodrębniamy nakładające się fragmenty między różnymi zestawami DMS i obliczamy wartości P mierzące prawdopodobieństwo uzyskania tych nakładających się fragmentów przez przypadek, stosując ponowne próbkowanie (resampling) [@fagny2015epigenomic]. Ponieważ poziomy metylacji DNA w docelowych miejscach są silnie skorelowane w obrębie regionów o określonej wielkości wynoszącej około 2000 pz, dla każdej listy DMS losowo pobieramy ponownie taką samą liczbę spośród wszystkich miejsc, biorąc pod uwagę faktyczną odległość chromosomalną między poszczególnymi DMS [@fagny2015epigenomic].

<!-- KOMENTARZ @maxi7524: dodałem format latex i dodałem fragmenty trochęzmieniłem format, żeby był bardziej czytelny -->

##### Analiza funkcjonalna i ontologia genów

W tym kroku wyodrębniamy wszystkie geny o zróżnicowanym stopniu metylacji, definiowane jako geny posiadające co najmniej jeden zidentyfikowany DMS [@fagny2015epigenomic]. Do przeprowadzenia analizy nadreprezentacji kategorii ontologii genów (GO) wśród wyselekcjonowanych genów wykorzystujemy pakiet `goseq` z biblioteki R Bioconductor [@fagny2015epigenomic]. Liczbę sond odpowiadających każdemu genomowi wprowadzimy do funkcji ważenia prawdopodobieństwa pakietu `goseq` w celu eliminacji błędu wynikającego z różnej długości genów [@fagny2015epigenomic]. 

<!-- #TODO @paulinaniemczak: ta część będzie do poprawy ALE to wymaga części marcina ze współćzesnym mapowaniem, tego nie mogę na ten momnet porpawić. Wydaje mi się że powinniśmy mieć pełne tło.  -->
Ponieważ nie wszystkie geny są reprezentowane przez , zestaw referencyjny (tło analizy) składa się wyłącznie z genów, dla których dostępne są poprawne dane z sond mikromacierzowych [@fagny2015epigenomic]. Zbiory DMS uznaje się za istotnie wzbogacone w danej kategorii funkcjonalnej, jeśli wartość P skorygowana o współczynnik fałszywych odkryć (FDR) wynosi maksymalnie 0,05 [@fagny2015epigenomic].

<!-- KOMENTARZ @maxi7524: fragment z populacją antyczną przeniosłem do `report/parts/methodology/dms_ancient.md` -->
