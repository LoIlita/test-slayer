# test-slayer

To jest **repo testowe**. Pokazuje, jak wyglądałaby tablica zadań zespołu benchmarkowego Slayer 1B, gdyby prowadzić ją na GitHubie zamiast na tablicy Zadań w appce wiedza. Kodu tu nie ma i nie będzie.

## Skąd są zadania

Wszystkie issues to kopie otwartych zadań projektu `slayer-1b` z workspace'u `fabryka-ai` (kolumny Todo i In Progress, stan z 31.10.2026). Na dole każdego issue stoi numer oryginału: `Źródło: tablica wiedzy fabryka-ai #N`. Źródłem prawdy pozostaje tablica wiedzy: zmiany tutaj nie wracają tam.

## Jak czytać

- **Grupa = milestone.** Każdy milestone to kamień milowy projektu: tytuł to stan rzeczy, który chcemy osiągnąć, opis to kryterium „jak poznasz, że zrobione”, termin to data kamienia. Zadania bez kamienia nie mają milestone'u; te odłożone mają etykietę `pozniej`.
- **Waga = etykieta `waga:5` … `waga:1`.** 5 = najważniejsze (czerwona), 1 = najmniej ważne (szara). Waga pochodzi z priorytetu zadania na tablicy wiedzy.
- **Kolejność = numer issue.** Issues założono od najważniejszych: najpierw waga malejąco, potem termin kamienia rosnąco (kamienie bez terminu na końcu), potem numer zadania. Issue #1 to więc pierwsze do wzięcia.
- **Pozostałe etykiety:** `dev` albo `research` (rodzaj zadania), `w-toku` (ktoś już nad tym pracuje), `pozniej` (odłożone, poza bieżącymi kamieniami).
