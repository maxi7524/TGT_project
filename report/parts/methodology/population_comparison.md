
#### Oznaczenia

Niech $K$ oznacza liczbę zidentyfikowanych DMS.

Dla każdego locus $j \in \{ 1, \dots, K \}$ wyznaczamy referencyjne częstości metylacji na podstawie wyników z analizy DMS dla populacji łowców ($p_{j}^{\mathrm{HG}} \in [0, 1]$) oraz rolników ($p_{j}^{\mathrm{AGR}} \in [0, 1]$).

Dla badanej nieznanej populacji kopalnej $X$ w wyniku map metylacyjnych otrzymujemy poziom metylacji dla każdego locus $j$, oznaczany jako $\hat{m}_{j} \in [0, 1]$.

#### Test klasyfikacyjny

Test klasyfikacyjny ma za zadanie sprawdzić, czy dana populacja jest istotnie podobna do populacji agrarnej (AGR) lub tradycyjnej grupy łowiecko-zbierackiej (HG).

Statystykę testową $\mathrm{D}_{\mathrm{EPM}}$ (Weighted Epigenetic Profile Matching Statistic) definiujemy jako:

$$
\mathrm{D}_{\mathrm{EPM}}(X) = \sum_{j=1}^K w_j \left[ \left( \hat{m}_j - p_j^{\mathrm{AGR}} \right)^2 - \left( \hat{m}_j - p_j^{\mathrm{HG}} \right)^2 \right]
$$

Gdzie:
- $w_{j} = \lvert p_{j}^{\mathrm{HG}} - p_{j}^{\mathrm{AGR}} \rvert$ mówi o tym, jak dany locus jest informatywny.
- $\mathrm{D}(X) < 0$ jest klasyfikowana jako populacja łowców.
- $\mathrm{D}(X) > 0$ jest klasyfikowana jako populacja rolników.

Żeby sprawdzić istotność tej zmiany, definiujemy następujące hipotezy:
- $H_{0}$: profil metylacji nie wykazuje specyficznego ukierunkowania.
- $H_{1}$: profil metylacji wskazuje istotne podobieństwo do jednej z grup.

Oraz stosujemy testy permutacyjne względem wartości $\hat{m}_{j}$.

#### Testy profilowe

Testy profilowe mają za zadanie identyfikację, czy istotnie występuje metylacja regionów (zbioru DMS) o określonym znaczeniu biologicznym. Regiony te będziemy nazywać grupami funkcjonalnymi, jest to podzbiór zidentyfikowanych loci. Niech $\mathcal{G}$ oznacza zbiór grup funkcjonalnych.

Dla każdej grupy $G \in \mathcal{G}$ definiujemy statystykę testową $\Lambda_{G}$ jako:

$$
\Lambda_G(X) = \frac{\sum_{j \in G} (\hat{m}_j - p_j^{\mathrm{AGR}})^2 - \sum_{j \in G} (\hat{m}_j - p_j^{\mathrm{HG}})^2}{\sum_{j \in G} (p_j^{\mathrm{HG}} - p_j^{\mathrm{AGR}})^2} \in [-1, 1]
$$

Gdzie skala opisuje bliskość względem populacji łowców i populacji rolników.

Żeby sprawdzić istotność każdej z tych zmian, definiujemy następujące hipotezy:
- $H_{0}^{G}$: profil metylacji grupy funkcjonalnej $G$ nie wykazuje specyficznego ukierunkowania.
- $H_{1}^{G}$: profil metylacji grupy funkcjonalnej $G$ wskazuje istotne podobieństwo do jednej z grup.

Oraz stosujemy testy permutacyjne względem zbioru $G$, biorąc losowe DMS.

