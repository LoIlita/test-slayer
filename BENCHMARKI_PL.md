# Które benchmarki przetłumaczyć, a które zbudować od zera

Każdy benchmark powstał w swoim kraju i niesie jego kontekst: amerykańskie prawo, historię, wyszukiwania Google z USA, angielską gramatykę. Model dla Polski ma znać polski teren, więc część testów da się przenieść tłumaczeniem, a część trzeba zbudować od zera z polskiej treści. Zasada i procedura: [METODOLOGIA.md](METODOLOGIA.md), sekcja D1.

Tabela niżej to **wstępny podział 38 angielskich testów ze Space** (T01–T50, `tasks_50.json`). To ocena z opisu zbiorów, nie pomiar: każdy wiersz potwierdzasz próbką 50 przykładów z sekcji D1, zanim zaczniesz pracę.

## Przetłumaczyć 1:1 (treść niezależna od kraju)

| T | benchmark | uwaga |
|---|---|---|
| T33 | GSM8K | zadania tekstowe z matematyki; podmień waluty i imiona |
| T34 | MATH | matematyka konkursowa |
| T19 | MathQA | matematyka |
| T18 | LogiQA | logika; źródło to chińskie egzaminy urzędnicze, sprawdź próbkę |

## Przetłumaczyć i sprawdzić próbkę (wiedza uniwersalna)

| T | benchmark | uwaga |
|---|---|---|
| T02 | ARC Challenge | nauki ścisłe, szkoła w USA; jednostki imperialne do zamiany |
| T08 | OpenBookQA | nauki ścisłe |
| T09 | SciQ | nauki ścisłe |
| T06 | PIQA | fizyczny zdrowy rozsądek; część przedmiotów z amerykańskiego domu |
| T20 | PROST | rozumowanie fizyczne |
| T05 | HellaSwag | codzienne czynności z WikiHow i ActivityNet; część zwyczajów z USA |
| T11 | COPA | przyczyna i skutek |
| T12, T15, T16 | CommitmentBank, RTE, ANLI | wnioskowanie z tekstu |
| T40, T41 | RACE, CoQA | czytanie ze zrozumieniem |
| T44, T45 | PubMedQA, MedMCQA | medycyna; MedMCQA pochodzi z indyjskich egzaminów, nazwy leków i procedury bywają lokalne |
| T47 | BIG-Bench Hard | mieszany: część podzadań jest językowa (np. kolejność przymiotników, sortowanie słów), te do budowy od zera |
| T36 | IFEval | część instrukcji dotyczy angielskiego (wielkie litery, język odpowiedzi), te do przepisania |

## Zbudować od zera z polskiego kontekstu (wiedza lokalna)

| T | benchmark | dlaczego | polski odpowiednik już w Space |
|---|---|---|---|
| T32 | MMLU | całe działy o USA: historia, prawo, ustrój, polityka zagraniczna | T37 LLMzSzŁ (polskie egzaminy państwowe); spoza Space: PLCC (niżej) |
| T35 | TruthfulQA | mity i przekonania popularne w USA, amerykańskie prawo | brak |
| T43 | TriviaQA | quizy z kultury i historii USA i Wielkiej Brytanii | T25 CzyWiesz, T30 PolQA |
| T42 | Natural Questions | prawdziwe wyszukiwania Google z USA | T30 PolQA, T31 PoQuAD |
| T46 | Social IQa | normy społeczne opisane przez ludzi z USA | brak |
| T39 | DROP | Wikipedia: mecze NFL, historia USA | brak |
| T10 | BoolQ | pytania z wyszukiwarki w USA | brak |
| T50 | ReCoRD | wiadomości CNN i Daily Mail | brak |
| T38 | SQuAD 2.0 | Wikipedia; odpowiedź to fragment tekstu, który po tłumaczeniu się rozjeżdża | T31 PoQuAD |
| T21 | SST-2 | recenzje filmów z USA | T22 PoLEMo2 |

## Zbudować od zera na polszczyźnie (zjawiska językowe)

| T | benchmark | dlaczego | polski odpowiednik już w Space |
|---|---|---|---|
| T01 | BLiMP | pary zdań z angielskiej gramatyki | T04 MultiBLiMP-PL |
| T49 | CoLA | poprawność gramatyczna angielskich zdań | brak |
| T07, T14 | WinoGrande, WSC | rozwiązywanie zaimków; w polskim rodzaj gramatyczny często zdradza odpowiedź | brak |
| T13 | WiC | znaczenia angielskich słów w kontekście | brak |
| T48 | PAWS | parafrazy ze zmienionym szykiem angielskiego zdania | T26 Polish Paraphrase Corpus |
| T17 | LAMBADA | ostatnie słowo z angielskich powieści | brak; materiał z polskiej literatury |
| T03 | WikiText-2 | perpleksja angielskiej Wikipedii | held-out PL (zadanie #10) |

## Polskie benchmarki od zera: kierunek długoterminowy

Polski odpowiednik nie musi naśladować amerykańskiego tematu. Budowa od zera to szansa na testy, których nikt jeszcze nie ma:

- **wiedza o Polsce:** historia, geografia, prawo, instytucje, kultura, język urzędowy, także lokalnie (np. Warszawa i region);
- **świeży tekst:** zbiory z polskich tekstów powstałych po dacie danych treningowych, odporne na kontaminację (wzór: UncheatableEval, zadanie #24);
- **zjawiska polszczyzny:** przypadki, aspekt, rodzaj, szyk swobodny, frazeologia.

### Wzór: PLCC

Polish Linguistic and Cultural Competency Benchmark (Dadas i in., 2025, https://arxiv.org/abs/2503.00995, wyniki: https://huggingface.co/spaces/sdadas/plcc) to gotowy przykład benchmarku zbudowanego od zera z polskiego kontekstu:

- 600 pytań napisanych ręcznie, 6 kategorii po 100: historia, geografia, kultura i tradycja, sztuka i rozrywka, gramatyka, słownictwo;
- pytania otwarte, model odpowiada tekstem; ocena regułami bez modelu-sędziego: lista dopuszczalnych odpowiedzi ze wszystkimi odmianami („bikiniarze”, „bikiniarzy”, „bikiniarstwo”), lista zakazanych, przy sortowaniu sprawdzana kolejność;
- jawne jest 60 pytań, reszta jest ukryta, żeby nie trafiła do danych treningowych;
- wynik to trafność per kategoria; przy 100 pytaniach SE ≈ 5 pkt, więc różnice między kategoriami jednego modelu poniżej ~10 pkt to szum.

Co z tego wynika przy pomiarze Slayera:

- PLCC jest polskim odpowiednikiem wiedzy lokalnej z tabeli wyżej (obok LLMzSzŁ). Etapy A–B z METODOLOGIA.md dotyczą go tak samo: odtwórz wynik modelu z tabeli PLCC, zanim policzysz Slayera. Najmniejszy model w tabeli to Bielik-1.5B-v3.0-Instruct.
- Pytania są pisane dla modeli instrukcyjnych („Podaj pojedynczą liczbę…”). Model bazowy 1B potrzebuje przykładów w prompcie; to odstępstwo od oryginału zapisujesz w karcie.
- Samodzielnie da się policzyć tylko 60 jawnych pytań (SE ≈ 6 pkt dla całości). Pełne 600 pytań mają autorzy; jak policzyć na nich Slayera, trzeba ustalić z nimi.

Przy budowie własnego zbioru bierz z PLCC: kategorie polskiego terenu, odpowiedzi jako listy form fleksyjnych (polska odmiana psuje proste porównanie tekstu), ocenę deterministyczną bez modelu-sędziego i ukrytą część zbioru.

Każdy taki zbiór powstaje tą samą metodą co oryginał, którego miejsce zajmuje (format, metryka, agregacja, n dające SE nie gorsze niż oryginał), i ma kartę z sekcji A METODOLOGIA.md. Zanim zaczniesz budować, sprawdź, czy polski odpowiednik już istnieje (kolumna wyżej, Open PL LLM Leaderboard: https://huggingface.co/spaces/speakleash/open_pl_llm_leaderboard).
