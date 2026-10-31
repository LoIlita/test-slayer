# test-slayer

To jest **repo testowe**: tablica zadań zespołu benchmarkowego Slayer 1B prowadzona na GitHubie, https://github.com/users/LoIlita/projects/2. Kodu tu nie ma i nie będzie. Kod, wyniki i PR-y są w https://github.com/slayerlabs/benchmarks-fabryka-ai, gałąź `feature/phase-0-foundation`, katalog `research/slayer-1b/`.

Kontakt w zespole: Discord.

Z Backlog do Ready przenosi zadanie Lolita (LoIlita) albo Elwenor, gdy opis jest kompletny.

Ten plik zbiera wszystko, co wspólne dla zadań. Zadanie odsyła tu po nazwie sekcji, zamiast powtarzać. Ścieżki plików w zadaniach są względne wobec https://github.com/slayerlabs/benchmarks-fabryka-ai/tree/feature/phase-0-foundation/research/slayer-1b.

## Jak wziąć zadanie

1. **Wybierz zadanie.** Pierwszy raz w labie? Widok **Start here** (etykieta `good first issue`: zawsze Exact i S). Masz godzinę lub dwie? Widok **Quick tasks** (Size S). Szukasz swojej dziedziny? Widok **By area**. Pole **Waga** mówi, co pilniejsze (5 = najpilniejsze).
2. **Ready → In Progress.** Przypisz się do zadania i przestaw Status na In Progress. Jedno zadanie naraz.
3. **In Progress → Review.** Otwórz PR w `slayerlabs/benchmarks-fabryka-ai` z gałęzi `research/bench-<nazwa>` do `feature/phase-0-foundation`. W opisie PR napisz `Refs LoIlita/test-slayer#N` (pełna nazwa repo, bo PR jest w innym repo niż zadanie; nie `Closes`). Zadanie zamyka recenzent, nie merge. Recenzję robi inna osoba: odtwarza Twój wynik, a nie tylko czyta kod.
4. **Review → Independent Check.** Tylko zadania z etykietą `needs-recheck`. Druga osoba liczy wynik od zera, na innym sprzęcie, bez zaglądania w Twoje liczby.
5. **Done.** Wynik bierze zespół modeli (widok **For model team**). Recenzent zamyka zadanie po Review albo po Independent Check.

## Pola i etykiety

- **Area:** Data, Architecture & Learning, Training Systems, Infrastructure, Evaluation & Measurement.
- **Mode:** `Exact` = dokładne kroki i definicja gotowego, dobre dla osób z doskoku; `Open` = problem do przemyślenia.
- **Size:** `S` do 2 h, `M` jeden wieczór, `L` kilka dni.
- **Waga:** 1–5, kolejność ważności. 5 = blokuje kamień o najbliższym terminie albo wszystkie benchmarki.
- **Grupa = kamień milowy (milestone)**, do którego zadanie prowadzi (sekcja „Kamienie i terminy”).
- **`good first issue`:** pierwsze zadanie w labie, zawsze Exact i S.
- **`needs-recheck`:** wynik idzie dalej (do Space albo do decyzji o treningu), więc musi przejść Independent Check.

## Kamienie i terminy

| kamień (Grupa i milestone) | termin | osiągnięty, gdy |
|---|---|---|
| Space Slayer1B-Benchmarks: wszystkie 50 testów mają wynik Slayera 1.06 | 8.11.2026 | w `results.json` Space żaden test nie ma `not_run` dla Slayera 1.06 (dziś 7), każdy nowy wynik ma CI95 wg `METHOD.md` |
| Pipeline ewaluacji zgodny ze Space i odtwarzalny przez nową osobę | 15.11.2026 | nowa osoba odtwarza ewaluację Slayera 1.06 z README repo fabryki bez pomocy; Pythia-1B na tym samym kodzie wychodzi w ±2 pkt od wartości referencyjnych; różnica WikiText ze Space wyjaśniona |
| Rdzeń monitoringu treningu w repo fabryki | 15.11.2026 | 8 benchmarków jest „zrobionych”: held-out NLL EN/PL/code, WikiText-2, ARC-Easy, HellaSwag, BLiMP, PIQA, BoolQ, WinoGrande |
| Druga kolejność benchmarków w repo fabryki | 30.11.2026 | 6 benchmarków jest „zrobionych”: ARC-Challenge, SciQ, OpenBookQA, LAMBADA, BLiMP Supplement, EWoK |
| Rozwój ewaluacji: świeży tekst | 15.12.2026 | UncheatableEval daje wynik Slayera 1.06 z SE, bramka liczb kod 0 |
| Zbiory benchmarkowe opisane: baza, format, n, SNR, kontaminacja | bez terminu | każdy test ma zapisaną bazę losową i większościową, format, n, zmierzony SNR i status kontaminacji |
| Dane treningowe: każdy składnik i kandydat sprawdzony w rejestrze | bez terminu | każdy składnik `slayer-pl-8x3b` i każdy kandydat w `dane/REJESTR_DATASETOW.md` ma potwierdzoną licencję, dedup, dekontaminację i rozmiar części PL |
| Ewaluacja checkpointów: każdy nowy checkpoint ma metryki ciągłe z SE | bez terminu | każdy nowy checkpoint dostaje automatycznie BPB i wynik po treści z SE, krzywa jest opublikowana |
| Trening i ablacje: B2/B3/B4 mają werdykt wobec progów | bez terminu | B2, B3 i B4 mają werdykt wobec progów z `PROGI.md` zapisanych przed pomiarem, wniosek o mieszance danych trafił do Arka |
| Start nowych osób i źródła: nowa osoba dochodzi do pierwszego PR | bez terminu | README repo fabryki i README tego repo prowadzą nową osobę do pierwszego PR, każde śledzone źródło ma właściciela i rytm przeglądu |

## Definicja „zrobiony” (benchmark)

Benchmark jest zrobiony, gdy jeden PR do `feature/phase-0-foundation` zawiera:

1. wpis w `slayer_eval/zadania.py` i klucz w słowniku `ZADANIA` w `slayer_eval/hipoteza_formatu.py`;
2. próbkę `slayer_eval/testy/dane/<nazwa>.jsonl` i test `tests/test_<nazwa>.py`, który przechodzi bez sieci (`.venv-eval/bin/python tests/test_<nazwa>.py`);
3. przebieg Slayera 1.06 na splicie jak w Space, z n i SE;
4. przebieg Pythii-1B tym samym skryptem i kontrolę (sekcja „Kontrola na Pythii-1B”);
5. różnicę Slayer − Pythia w SE z werdyktem (sekcja „SE i werdykt”);
6. bazę losową i większościową oraz rozkład odpowiedzi (dla testów z wyborem odpowiedzi), z `wyniki/raport_formatu.json`;
7. liczby w `wyniki/LICZBY.json` i bramkę liczb z kodem 0 (sekcja „Bramka liczb”);
8. `Refs LoIlita/test-slayer#N` w opisie PR i link do PR w komentarzu zadania.

Wzór kompletnego benchmarku: PR https://github.com/slayerlabs/benchmarks-fabryka-ai/pull/9 (BLiMP), gałąź `research/bench-blimp`: `slayer_eval/blimp.py`, `tests/test_blimp.py`, `wyniki/blimp/THRESHOLDS.md` (progi zapisane przed pomiarem), `wyniki/blimp/BLIMP.md`, surowe wyniki `wyniki/blimp/raw/*.jsonl.gz`.

Pełny zbiór czy próbka 200: na razie liczysz pełny zbiór (jak BLiMP w PR #9), a zadania kamienia Space dodatkowo próbkę Space. Czy pełny zbiór jest wymagany zawsze, ustalimy na spotkaniu.

## Model i uruchomienie

- **Slayer 1.06:** `SlayerLab/Slayer1B-1.06`, rewizja `ef81be58742544ad32e1afab38db011fec209609` (krok 13848, czyli `ckpt_13848`). Ładujesz go tylko przez `slayer_eval/slayer_model.py`. HFLM i AutoModel kończą się `ValueError: Unrecognized model`, bo `config.json` Slayera nie ma `model_type`.
- **Pythia-1B (kontrola):** `EleutherAI/pythia-1b`, rewizja `f73d7dcc545c8bd326d8559c8ef84ffe92fea6b2`.
- **LFM2.5 (drugi model w Space):** `LiquidAI/LFM2.5-1.2B-Base`, rewizja `7453bca97ca1e67754c4035a4b4c584e1c9dd725`.
- **Checkpointy:** https://huggingface.co/SlayerLab/Slayer1B-1.00-checkpoints, https://huggingface.co/SlayerLab/Slayer1B-1.04-checkpoints (pliki `.pt`, `ckpt_13848.pt` = 1.06), https://huggingface.co/SlayerLab/Slayer1B-1.06-checkpoints.
- **Dane treningowe:** https://huggingface.co/datasets/SlayerLab/slayer-pl-8x3b (web 63%, core 19%, legal 8%, EN 10%), https://huggingface.co/datasets/SlayerLab/polish-dynaword.

### Przygotowanie (raz)

Raz na komputer, z siecią do Hugging Face (HF). Kilka GB dysku; działa na CPU (wolno) albo GPU (`--device cuda`).

```bash
git clone -b feature/phase-0-foundation https://github.com/slayerlabs/benchmarks-fabryka-ai
cd benchmarks-fabryka-ai/research/slayer-1b
python3 -m venv .venv-eval
.venv-eval/bin/pip install -r slayer_eval/requirements.lock.txt pytest
.venv-eval/bin/python slayer_eval/pobierz_zbiory.py
.venv-eval/bin/huggingface-cli download SlayerLab/Slayer1B-1.06 --revision ef81be58742544ad32e1afab38db011fec209609 --local-dir models/slayer-1.06
.venv-eval/bin/huggingface-cli download EleutherAI/pythia-1b --revision f73d7dcc545c8bd326d8559c8ef84ffe92fea6b2 --local-dir models/pythia-1b
.venv-eval/bin/python tests/test_slayer_eval.py
```

Wszystkie komendy w zadaniach uruchamiasz w `benchmarks-fabryka-ai/research/slayer-1b/`.

### Uruchomienie

Slayera i Pythię liczy ten sam skrypt, `slayer_eval/hipoteza_formatu.py`:

```bash
.venv-eval/bin/python slayer_eval/hipoteza_formatu.py licz --model models/slayer-1.06 --zadania arc_c_cloze,arc_c_mc --wyjscie wyniki/1.06 --device cpu
.venv-eval/bin/python slayer_eval/hipoteza_formatu.py licz --model models/pythia-1b --zadania arc_c_cloze,arc_c_mc --wyjscie wyniki/pythia-1b --device cpu
.venv-eval/bin/python slayer_eval/hipoteza_formatu.py analiza --wyniki wyniki
```

- `licz` zapisuje jeden plik `<zadanie>.jsonl` na zadanie; przerwane liczenie wznawia się od brakujących pytań.
- `analiza` zapisuje `wyniki/raport_formatu.json`: n, SE, bazy, rozkład odpowiedzi i porównanie modeli.
- Nazwy dla `--zadania` to klucze słownika `ZADANIA` w `slayer_eval/hipoteza_formatu.py` (m.in. `arc_c_cloze`, `arc_c_mc`, `arc_c_mc_25shot`, `arc_e_cloze`, `arc_e_mc`, `siqa_cloze`, `siqa_mc`, `med_cloze`, `mmlu_cloze`, `belebele_cloze`, `multiblimp`). Nowy benchmark dodajesz jako nowy klucz.
- Slayera ładuje tylko `slayer_eval/slayer_model.py`. Zwykłe ładowanie z biblioteki transformers kończy się błędem `ValueError: Unrecognized model`.
- WikiText-2 Slayera: `.venv-eval/bin/python slayer_eval/hipoteza_formatu.py wikitext --model models/slayer-1.06 --wyjscie wyniki/1.06 --device cpu`.
- Wynik Pythii w formacie Space (`results.json`) daje `ZADANIA=T03 PROBKA=pelna ETYKIETA=pythia-1b slayer_eval/ewaluuj_slayer.sh EleutherAI/pythia-1b f73d7dcc545c8bd326d8559c8ef84ffe92fea6b2` (`ZADANIA` = numery T z `slayer_eval/zadania.py`, `PROBKA` = `pelna` albo `panel200`).
- Różnice wobec Space: lm-eval 0.4.9.1 z PyPI zamiast commitu `6d2abda4`; próbka 200 w repo fabryki może nie być identyczna z próbką Space.

## Bramka liczb

Każda liczba w raporcie pochodzi z `wyniki/LICZBY.json`, który tworzy `narzedzia/liczby.py` z plików wyników (m.in. `wyniki/raport_formatu.json`). Przed każdym PR:

```bash
.venv-eval/bin/python narzedzia/liczby.py && .venv-eval/bin/python narzedzia/sprawdz_raporty.py
```

Kod 0 = każda liczba w raportach jest w `LICZBY.json`, a każde porównanie ma SE i werdykt. Testy samej bramki: `.venv-eval/bin/python tests/test_liczby_raportow.py`. Dziś bramka na gałęzi `feature/phase-0-foundation` kończy się kodem 1 przez raporty EWoK-PL; naprawa to zadanie #40.

## SE i werdykt

- **SE** (błąd standardowy): dla trafności p na n pytaniach SE = √(p(1−p)/n). Przedział 95% (CI95) ≈ wynik ± 1,96·SE. Space liczy CI95 bootstrapem (10 000 powtórzeń, seed 20261028).
- **Porównanie dwóch wyników:** różnica, SE różnicy (parowane, gdy oba modele odpowiadały na te same pytania; inaczej √(SE₁² + SE₂²), co zawyża szum), z = różnica / SE.
- **Werdykt** (jak w `narzedzia/liczby.py`): |z| ≥ 3 „pewne”, 2–3 „prawdopodobne”, < 2 „szum”. „Szum” znaczy, że pomiar nie rozstrzyga, a nie że różnicy nie ma.
- Najpierw odtwórz znany wynik (kontrola na Pythii-1B), potem mierz Slayera. Każda liczba w raporcie ma n i SE; każde porównanie ma różnicę w SE i werdykt.

## Bazy: losowa i większościowa

- **Baza losowa:** wynik zgadywania (1/k przy k opcjach, 50% przy parach zdań).
- **Baza większościowa:** wynik stałej odpowiedzi „najczęstsza klasa”, np. zawsze „tak” w BoolQ; tylko dla testów o stałym zbiorze klas.
- **Udział najczęstszej predykcji:** jaki odsetek odpowiedzi modelu to ta sama opcja. Slayer i Pythia-1B wybierają jedną literę w 84–96% pytań z literami, więc wynik bez tych liczb myli.

## Space Slayer1B-Benchmarks

- https://huggingface.co/spaces/SlayerLab/Slayer1B-Benchmarks to statyczna strona z wynikami 50 benchmarków (T01–T50: 38 EN, 12 PL) dla Slayera 1.06 i LFM2.5-1.2B-Base. Sama nic nie liczy, tylko wyświetla `results.json`.
- Pliki Space: https://huggingface.co/spaces/SlayerLab/Slayer1B-Benchmarks/blob/main/results.json (wyniki), https://huggingface.co/spaces/SlayerLab/Slayer1B-Benchmarks/blob/main/tasks_50.json (ustawienia testów), https://huggingface.co/spaces/SlayerLab/Slayer1B-Benchmarks/blob/main/METHOD.md (protokół; „§6” w zadaniach = sekcja 6 tego pliku).
- **Numery T (T01–T50):** numer testu w Space; ten sam numer ma wpis w `slayer_eval/zadania.py`.
- `tasks_50.json`: dla każdego Txx zadanie lm-eval, metryka, split, liczba przykładów w prompcie oraz repo i rewizja zbioru (pole `license.source_revisions`). `METHOD.md`: protokół (lm-eval commit `6d2abda4` dla EN, fork OpenPL `21d0ea9c` dla PL; float32, batch 1, kontekst do 4096 tokenów; EN 0-shot, PL 5-shot).
- **Próbka 200:** Space liczy każdy test na `min(200, N)` dokumentach wybranych po SHA-256 `20261028|benchmark_id|member_task|source_id`, tych samych dla wszystkich modeli; WikiText-2 w całości (62 dokumenty). Przy n = 200 SE ≈ 3,5 pkt.
- **Kto wgrywa wynik:** `results.json` eksportuje runner właściciela Space (Arek, SlayerLab). Zespół przekazuje mu wynik; forma przekazania do ustalenia na spotkaniu.
- **Bez wyniku Slayera 1.06 (`not_run`):** T06 PIQA, T14 WSC273, T18 LogiQA, T19 MathQA, T36 IFEval, T44 PubMedQA, T46 Social IQa.
- Historia wyników wersji 1.00–1.06: https://huggingface.co/spaces/SlayerLab/Slayer1B (`scores-history.csv`).

## Kontrola na Pythii-1B

Pythia-1B to model kontrolny tej samej skali. Liczysz go tym samym kodem co Slayera. Jeśli wynik Pythii odbiega od opublikowanego, błąd jest w kodzie ewaluacji w repo fabryki, nie w Slayerze.

- Wartości opublikowane (0-shot, krok 143000): https://github.com/EleutherAI/pythia/blob/main/evals/pythia-v0/pythia-1b/zero-shot/pythia-1b-0shot-global_step143000_eval_results_11-28-2022-15-14-35.json

| benchmark | acc | acc_norm |
|---|---|---|
| ARC-Easy | 0,5585 | 0,5004 |
| ARC-Challenge | 0,2440 | 0,2722 |
| SciQ | 0,843 | 0,761 |
| PIQA | 0,704 | 0,7051 |
| WinoGrande | 0,5375 | — |
| LAMBADA (openai) | 0,5622 | — |
| LogiQA | 0,2258 | 0,2873 |
| BLiMP (średnia paradygmatów) | 0,8234 | — |

- Plik ma `wsc` z SuperGLUE, a Space liczy `wsc273`: to nie jest referencja dla T14.
- **Kryterium ±2 pkt dotyczy tylko trafności (acc, acc_norm).** Dla BPB, perpleksji i benchmarków bez opublikowanej wartości kryterium ustala zadanie #41.

## Słowniczek

- **acc:** trafność; model wybiera odpowiedź o największym log-prawdopodobieństwie.
- **acc_norm:** jak acc, ale log-prawdopodobieństwo dzielone przez długość odpowiedzi, żeby długie odpowiedzi nie przegrywały z krótkimi.
- **cloze i MC z literami:** cloze = model ocenia treść każdej odpowiedzi doklejoną do pytania; MC z literami = odpowiedzi podpisane A/B/C/D i model ocenia samą literę. Małe modele w MC z literami wybierają zwykle jedną literę.
- **BPB** (bity na bajt): koszt tekstu albo poprawnej odpowiedzi w bitach na bajt; niżej = lepiej; porównywalne między modelami o różnych tokenizerach.
- **PMI:** log P(odpowiedź | pytanie) − log P(odpowiedź); usuwa z wyboru to, że jakaś odpowiedź jest po prostu częsta.
- **NLL:** strata, ujemny logarytm prawdopodobieństwa tekstu.
- **held-out:** tekst odłożony, którego model nie widział w treningu; strata na nim mierzy postęp bez progu losowości.
- **split:** część zbioru (train, validation, test); Space podaje, której używa.
- **kamień:** kamień milowy, stan rzeczy z kryterium i terminem; na tablicy pole Grupa i milestone.
- **checkpoint, `ckpt_13848`:** zapis wag po danym kroku treningu; `ckpt_13848` to Slayer 1.06.
- **wygaszanie (LR):** obniżanie tempa uczenia do zera na końcu treningu; krótki test od `ckpt_13848` dał −0,047 nat straty bez zmiany trafności.
- **ramię:** jeden wariant danych w eksperymencie, trenowany od tego samego checkpointu: C = obecna mieszanka, C2 = to samo w innej kolejności (miara szumu), E25 = 25% angielskiego zamiast 10%, PLHQ = polski web z FineWeb2-HQ zamiast HPLT.
- **SNR:** stosunek sygnału do szumu; czy test rozróżnia checkpointy ponad przypadkowe wahania.
- **kontaminacja:** pytania testu obecne w danych treningowych; Space sprawdza ją 13-gramami słów (`METHOD.md` §6).
- **13-gram:** ciąg 13 kolejnych słów; wspólny 13-gram testu i danych treningowych liczy się jako trafienie.
- **dekontaminacja, dedup:** usunięcie z danych treningowych tekstów testów; usunięcie powtórzonych dokumentów.
- **HF (Hugging Face):** serwis z modelami i zbiorami; **repo HF** to `właściciel/nazwa`, **rewizja** to hash commitu repo.
- **parquet, `refs/convert/parquet`:** format plików zbioru; dla zbiorów ze skryptem ładującym HF trzyma gotowe pliki parquet na gałęzi `refs/convert/parquet` repo zbioru.
- **lm-eval:** biblioteka EleutherAI do ewaluacji modeli (`lm_eval`); Space liczy nią testy angielskie.
- **CI95:** przedział ufności 95% (sekcja „SE i werdykt”).
- **PMI, BPB poprawnej odpowiedzi, „po treści”:** wybór odpowiedzi według treści zamiast litery; w `wyniki/raport_formatu.json` jako acc PMI i acc według BPB.
- **pary minimalne:** dwa zdania różniące się jednym słowem, jedno poprawne; model ma dać poprawnemu wyższe prawdopodobieństwo (BLiMP).
- **HPLT, FineWeb2-HQ:** zbiory polskiego tekstu z internetu; HPLT jest w danych Slayera, FineWeb2-HQ to kandydat.
- **PII:** dane osobowe w tekście (nazwiska, adresy, numery).
- **gałąź PR, Draft:** gałąź, z której otwierasz PR; Draft = PR oznaczony jako niegotowy do recenzji.
- **Review, Independent Check:** sekcja „Jak wziąć zadanie”, punkty 3–4.

## Kody zadań z listy dla Arka

Z https://github.com/slayerlabs/benchmarks-fabryka-ai/blob/feature/phase-0-foundation/research/slayer-1b/lab/DLA_ARKA_I_CHETNYCH.md, „Część 2. Zadania dla chętnych”:

- **A1** powtórzenie pomiaru formatu (ślepa paczka): #44
- **A2** przeciek testów w angielskiej części danych: #25
- **A3** PolQA w danych treningowych: zamknięte, Space ma pomiar (#28)
- **A4** licencje składników `slayer-pl-8x3b`: #29
- **A5** różnica WikiText ze Space: #3 (poprawka mianownika w PR slayerlabs/benchmarks-fabryka-ai#8, przegląd w #44)
- **A6** tokenizacja litery odpowiedzi („ A” i „A”): w #44
- **A7** kandydaci na dane: #30
- **B1** kiedy model uczy się odpowiadać literą: #31
- **B2** udział angielskiego 10% vs 25%, **B3** polski web HPLT vs FineWeb2-HQ: plan i progi w `testy_danych_slayer/`, recenzja planu w #44
- **B4** dłuższe wygaszanie LR: tylko jeśli krótki test pokaże spadek straty
- **K3** polskie testy z wyborem odpowiedzi liczone po treści: #44; **K2** polskie dane QA w wygaszaniu: dopiero po wyniku K3

B1–B4 wymagają karty graficznej i zgody Arka.

## Szablon zadania

Nowe zadanie zakładaj z szablonu „Zadanie” (`.github/ISSUE_TEMPLATE/zadanie.md`): cel w jednym zdaniu, kroki albo pytanie, dane i linki, „Gotowe, gdy” (wynik z SE, kontrola na Pythii-1B, PR z `Refs LoIlita/test-slayer#N`), kto sprawdza.
