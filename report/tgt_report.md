---
title: "IDMSpHGiAGR"
subtitle: "Identyfikacja DMS pomiędzy HG i AGR"
author: "Grupa 4"
date: "2026-06-24"
format:
  pdf:
    toc: true
    toc-depth: 3
    number-sections: true
    colorlinks: true
    fontsize: 11pt
    geometry:
      - top=25mm
      - bottom=25mm
      - left=20mm
      - right=20mm
    include-in-header:
      - text: |
          \usepackage{setspace}
          \setstretch{1.1}
          \usepackage{amsmath}
  html:
    toc: true
    toc-depth: 3
    number-sections: true
    theme: cosmo
    embed-resources: true
bibliography: references.bib
csl: ieee.csl
link-citations: true
---

{{< include parts/introduction/introduction.md >}}

***

{{< include parts/introduction/state_of_art.md >}}

***

## Metodologia

### Kohorty i liczba próbek

{{< include parts/methodology/cohorts.md >}}

***

### Mapy metylacyjne

{{< include parts/methodology/methylation_maps.md >}}

***

### Analiza DMS

{{< include parts/methodology/dms.md >}}

***

### Metoda statystyczna do porównywania populacji.

{{< include parts/methodology/population_comparison.md >}}

## Oczekiwane wyniki

### Cechy charakterystyczne

{{< include parts/results_and_problems/characteristics_features.md >}}


### Wpływ na tezy historyczne 

{{< include parts/results_and_problems/impact_on_historical_thesis.md >}}

### Możliwe problemy

{{< include parts/results_and_problems/problems.md >}}

<!-- Below will be bibliography -->