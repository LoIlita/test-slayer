# test-slayer

To jest **repo testowe**. Pokazuje, jak wyglądałaby tablica zadań zespołu benchmarkowego Slayer 1B prowadzona na GitHubie: https://github.com/users/LoIlita/projects/2. Kodu tu nie ma i nie będzie.

Issues to kopie otwartych zadań projektu `slayer-1b` z workspace'u `fabryka-ai` (stan z 31.10.2026). Na dole każdego issue stoi numer oryginału: `Źródło: tablica wiedzy fabryka-ai #N`. Źródłem prawdy pozostaje tablica wiedzy: zmiany tutaj nie wracają tam.

## Jak wziąć zadanie

1. **Wybierz zadanie.** Pierwszy raz w labie? Widok **Start here** (etykieta `good first issue`: zawsze Exact i S). Masz godzinę lub dwie? Widok **Quick tasks** (Size S). Szukasz swojej dziedziny? Widok **By area**.
2. **Ready → In Progress.** Przypisz się do issue i przestaw Status na In Progress. Jedno zadanie naraz.
3. **In Progress → Review.** Otwórz PR. W opisie PR odwołaj się do zadania przez `Refs #N`, **nie** `Closes #N`: zadanie zamyka recenzent, nie merge. Review robi inna osoba: odtwarza Twój wynik, a nie tylko czyta kod.
4. **Review → Independent Check.** Tylko zadania z etykietą `needs-recheck`. Druga osoba liczy wynik od zera, na innym sprzęcie, bez zaglądania w Twoje liczby.
5. **Done.** Wynik bierze zespół modeli (widok **For model team**). Recenzent zamyka issue po Review albo po Independent Check.

## Pola i etykiety

- **Area:** Data, Architecture & Learning, Training Systems, Infrastructure, Evaluation & Measurement.
- **Mode:** `Exact` = dokładne kroki i definicja gotowego, dobre dla osób z doskoku; `Open` = problem do przemyślenia.
- **Size:** `S` do 2 h, `M` jeden wieczór, `L` kilka dni.
- **`good first issue`:** pierwsze zadanie w labie, zawsze Exact i S.
- **`needs-recheck`:** wynik idzie dalej do modeli (np. liczby benchmarków do Space albo do decyzji o treningu), więc musi przejść Independent Check.
- **Grupa = kamień milowy (milestone)** projektu, do którego zadanie prowadzi.

## Szablon zadania

Nowe zadanie zakładaj z szablonu „Zadanie” (`.github/ISSUE_TEMPLATE/zadanie.md`): cel w jednym zdaniu, kroki albo pytanie, dane i linki, „Gotowe, gdy” (wynik z SE, kontrola na Pythii-1B ±2 pkt, PR), kto sprawdza.
