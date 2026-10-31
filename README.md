# test-slayer

Reguły i kod zespołu benchmarkowego są w [slayerlabs/benchmarks-fabryka-ai](https://github.com/slayerlabs/benchmarks-fabryka-ai): jak dołączyć i kiedy benchmark jest zrobiony w [README](https://github.com/slayerlabs/benchmarks-fabryka-ai/blob/main/README.md), jak powstaje benchmark w [METODOLOGIA.md](https://github.com/slayerlabs/benchmarks-fabryka-ai/blob/main/research/METODOLOGIA.md).

To repo trzyma tylko zadania (issues) i tablicę zespołu: https://github.com/users/LoIlita/projects/2. Kodu tu nie ma; PR-y idą do `main` w repo benchmarków. Nowa osoba zaczyna od #45.

## Jak wziąć zadanie

1. **Wybierz zadanie** na tablicy. Pierwszy raz? Widok [Start here](https://github.com/users/LoIlita/projects/2/views/4) (etykieta `good first issue`, zawsze Exact i S). Masz godzinę lub dwie? Widok [Quick tasks](https://github.com/users/LoIlita/projects/2/views/5) (Size S). Pole **Waga** mówi, co pilniejsze (5 = najpilniejsze).
2. **Ready → In Progress.** Przypisz się do zadania i przestaw Status. Jedno zadanie naraz.
3. **In Progress → Review.** PR według [CONTRIBUTING, „PR”](https://github.com/slayerlabs/benchmarks-fabryka-ai/blob/main/CONTRIBUTING.md#pr); w opisie `Refs LoIlita/test-slayer#N` (pełna nazwa repo, bo PR jest w innym repo niż zadanie).
4. **Review → Independent Check.** Tylko zadania z etykietą `needs-recheck`: druga osoba liczy wynik od zera, na innym sprzęcie.
5. **Done.** Zadanie zamyka recenzent, nie merge.

Z Backlog do Ready przenosi zadanie Lolita (LoIlita) albo Elwenor, gdy opis jest kompletny. Nowe zadanie zakładasz z szablonu „Zadanie”.

## Pola tablicy

- **Area:** Data, Architecture & Learning, Training Systems, Infrastructure, Evaluation & Measurement.
- **Mode:** `Exact` = dokładne kroki i definicja gotowego; `Open` = problem do przemyślenia.
- **Size:** `S` do 2 h, `M` jeden wieczór, `L` kilka dni.
- **Waga:** 1–5; 5 = blokuje kamień o najbliższym terminie albo wszystkie benchmarki.
- **Grupa** = kamień milowy zadania, z terminem i kryterium w opisie: https://github.com/LoIlita/test-slayer/milestones.
- Etykiety: [CONTRIBUTING, „Etykiety”](https://github.com/slayerlabs/benchmarks-fabryka-ai/blob/main/CONTRIBUTING.md#etykiety).

## Kody zadań z listy dla Arka

Z [`research/slayer-1b/lab/DLA_ARKA_I_CHETNYCH.md`](https://github.com/slayerlabs/benchmarks-fabryka-ai/blob/main/research/slayer-1b/lab/DLA_ARKA_I_CHETNYCH.md), „Część 2. Zadania dla chętnych”:

- **A1** powtórzenie pomiaru formatu (ślepa paczka): #44
- **A2** przeciek testów w angielskiej części danych: #25
- **A3** PolQA w danych treningowych: zamknięte, Space ma pomiar (#28)
- **A4** licencje składników `slayer-pl-8x3b`: #29
- **A5** różnica WikiText ze Space: wyjaśniona poprawką mianownika (`research/benchmarks/wikitext-2/`); braki etapu B w #3
- **A6** tokenizacja litery odpowiedzi („ A” i „A”): w #44
- **A7** kandydaci na dane: #30
- **B1** kiedy model uczy się odpowiadać literą: #31
- **B2** udział angielskiego 10% wobec 25%, **B3** polski web HPLT wobec FineWeb2-HQ: plan i progi w `research/slayer-1b/testy_danych_slayer/`, recenzja planu w #44
- **B4** dłuższe wygaszanie tempa uczenia: tylko jeśli krótki test pokaże spadek straty
- **K3** polskie testy z wyborem odpowiedzi liczone po treści: #44; **K2** polskie dane QA w wygaszaniu: dopiero po wyniku K3

B1–B4 wymagają karty graficznej i zgody Arka.
