# Prior art i uzupełnienia z doświadczenia ebooka

## Uzupełnienie sekcji "Racja kierunkowa jako jednostka wyszukiwania" — §7 Źródła zewnętrzne

*Sierpień 2026. Domyka lukę jawnie zaznaczoną w oryginalnym dokumencie:
„Linia atomowych faktów (…) do uzupełnienia o konkretne pozycje przed
publikacją."*

---

## 1. Metoda przeglądu

Sprawdzone niezależnie, oprócz Chena: (a) rhetorical role labeling w prawie,
(b) argument mining w orzecznictwie, (c) sieci cytowań z adnotacją kierunku
(precedent treatment), (d) narzędzia komercyjne 2024-2026 z jednostką
retrievalu poniżej dokumentu, (e) linia „atomic facts" w weryfikacji
faktograficznej jako sąsiednia (nieprawnicza) inspiracja.

## 2. Rhetorical Role Labeling — najbliższy istniejący nurt

Identyfikacja funkcji zdania w orzeczeniu (Facts / Issue / Arguments / Ratio /
Ruling by Precedent / Ruling by Present Court / Precedent / Statute) to
**dokładnie ten sam problem, który rozwiązuje warstwa „tryb" w §2** naszej
sekcji — z jedną różnicą, która czyni go niewystarczającym samodzielnie.

- Bhattacharya i in., *Identification of Rhetorical Roles of Sentences in
  Indian Legal Judgments* (2019, [arXiv:1911.05405](https://arxiv.org/abs/1911.05405))
  — pierwsza neuronowa klasyfikacja ról na poziomie zdania, korpus >7000
  orzeczeń Sądu Najwyższego Indii, >300 000 zdań, 7 ról.
- Malik i in. — rozszerzenie do 13 ról + multi-task learning.
- Bambroo i in., *MARRO: Multi-headed Attention for Rhetorical Role Labeling
  in Legal Documents* (2025, [arXiv:2503.10659](https://arxiv.org/pdf/2503.10659)).
- *LegalSeg: Unlocking the Structure of Indian Legal Judgments Through
  Rhetorical Role Classification* (2025, [arXiv:2502.05836](https://arxiv.org/pdf/2502.05836)).
- *Segment First, Retrieve Better: Realistic Legal Search via Rhetorical
  Role-Based Queries* (sierpień 2025, [arXiv:2508.00679](https://arxiv.org/html/2508.00679))
  — **najświeższa i koncepcyjnie najbliższa**: retrieval po segmentach
  retorycznych zamiast po całych dokumentach lub równych chunkach.

**Czego brakuje wobec racji kierunkowej.** Rola retoryczna mówi *czym jest*
zdanie (argumentem, ratio, faktem). Żadna z powyższych prac nie koduje **na
czyją korzyść** dany fragment został rozstrzygnięty, ani nie agreguje ról
ponad wieloma orzeczeniami w policzalny rozkład. Rola bez kierunku odpowiada
na „co to jest", nie na „kto wygrywa ten spór, i jak często".

## 3. Argument Mining w orzecznictwie — struktura bez kierunku

- Poudyal i in., *ECHR: Legal Corpus for Argument Mining*
  ([ACL 2020.argmining-1.8](https://aclanthology.org/2020.argmining-1.8/)) —
  42 orzeczenia ETPC, schemat premise/conclusion za modelem Waltona.
  Rozwinięcie do 373 orzeczeń / ~2,3 mln tokenów / >15 000 spanów argumentów
  ([Springer AI & Law 2023](https://link.springer.com/article/10.1007/s10506-023-09361-y)).
- Habernal, Gurevych i współautorzy — linia argument mining ogólna,
  adaptowana do prawa (m.in. *Mining Legal Arguments in Court Decisions*,
  [arXiv:2208.06178](https://arxiv.org/html/2208.06178)).

**Czego brakuje.** Struktura premise→conclusion mapuje **logikę** argumentu,
nie jego **status procesowy**. Brak rozróżnienia ratio/obiter, brak instancji
źródłowej, brak pozycji jako mechanizmu rekonstrukcji wywodu — te warstwy są
specyficznie nasze.

## 4. Precedent Treatment Classification — kierunek jest, ale na złym poziomie

To najbliższa istniejąca realizacja samej warstwy **kierunku**:

- Shepard's Citations / Westlaw KeyCite — kody `followed` / `distinguished` /
  `criticized` / `overruled`, komercyjnie od dekad.
  ([Thomson Reuters — Checking Cases with KeyCite](https://legal.thomsonreuters.com/blog/westlaw-tip-of-the-week-checking-cases-with-keycite/))
- *Validate Your Authority: Benchmarking LLMs on Multi-Label Precedent
  Treatment Classification* (2026, [arXiv:2605.17691](https://arxiv.org/pdf/2605.17691))
  — świeży benchmark LLM na klasyfikacji tych samych kodów; potwierdza wprost
  w treści, że klasyfikacja działa **na poziomie całego cytowania
  orzeczenie→orzeczenie**, nie pojedynczego argumentu wewnątrz orzeczenia,
  i **nie agreguje wyniku w rozkład** ponad wieloma sprawami.

**To jest kluczowy wynik przeglądu.** Zestawienie z §3 naszej sekcji: kierunek
w Shepard's/KeyCite żyje na poziomie dokumentu-jako-całości (orzeczenie A
„podąża za" orzeczeniem B). Nasza „racja kierunkowa" przenosi dokładnie tę
samą ideę **w dół, na poziom pojedynczego argumentu wewnątrz orzeczenia** —
i dopiero stamtąd pozwala zbudować oś sporną z rozkładem K:B. Żaden ze
sprawdzonych systemów, komercyjnych ani akademickich, tego kroku nie robi.

## 5. Structure-aware case retrieval — struktura dla dopasowania, nie dla ekstrakcji

SAILER ([arXiv:2304.11370](https://arxiv.org/abs/2304.11370)) i pochodne
(ReaKase, ([arXiv:2510.26178](https://arxiv.org/pdf/2510.26178)))
wykorzystują strukturę dokumentu (Procedure/Facts/Reasoning/Decision) jako
sygnał trenujący embedding **całego orzeczenia** pod kątem podobieństwa
case-do-case. To rozwiązuje inny problem niż nasz: „znajdź podobną sprawę",
nie „wyciągnij z tej sprawy argument z kierunkiem".

## 6. Narzędzia komercyjne 2024-2026 (Harvey, CoCounsel, vLex Vincent)

Sprawdzone przez benchmark Vals Legal AI Report (luty 2025) — porównanie
publiczne skupia się na trafności odpowiedzi na pytania (document Q&A: Harvey
94,8%, CoCounsel 89,6%), nie na architekturze jednostki retrievalu. Żaden
z dostawców nie publikuje opisu jednostki poniżej dokumentu niosącej kierunek
rozstrzygnięcia — to zamknięta architektura komercyjna, nie da się
zweryfikować z zewnątrz czy robią coś podobnego; z publicznych materiałów nie
wynika, żeby robili.

Analityka sporów (**Lex Machina**, **Trellis**) idzie inną drogą całkowicie:
przewiduje wynik z **metadanych** sprawy (sędzia, typ wniosku, kancelaria,
70-80% trafności), nie z treści argumentu. Nie ma tam jednostki „racja"
w ogóle — to model regresyjny na cechach ustrukturyzowanych, nie retrieval.

## 7. Linia sąsiednia (nieprawnicza): atomic facts w ocenie faktograficznej

Min i in., *FActScore* (2023) — dekompozycja długiej wypowiedzi na atomowe
fakty do niezależnej weryfikacji, każdy jako pojedyncze, minimalne twierdzenie.
Linia kontynuowana (VeriFastScore, DnDScore, OpenFActScore, 2024-2026).
To bezpośrednia inspiracja (i technicznie bliski krewny) propozycji Chena —
ale fakt w tej linii jest z definicji bezkierunkowy (weryfikuje się go jako
prawdziwy/fałszywy, nie jako argument jednej ze stron sporu). Cytowane jako
kontekst, nie jako prior art dla warstwy kierunku.

## 8. Wniosek przeglądu

Żadna ze sprawdzonych prac — akademickich ani komercyjnych — **nie łączy
kierunku, trybu, instancji źródłowej, pozycji i cytatu-kotwicy w jednej
jednostce, agregowalnej w policzalną oś sporną**. Trzy nurty trafiają
w pojedyncze warstwy z osobna (rhetorical roles → tryb, precedent treatment →
kierunek, structure-aware retrieval → pozycja jako sygnał treningowy), żaden
nie łączy ich w spójną, atomową jednostkę poniżej dokumentu. To wzmacnia
oryginalną tezę sekcji, nie jest listą przegapionych konkurentów.

---

## 9. Uzupełnienie z doświadczenia ebooka — propozycja do R-METODA (§6 `../SPEC.md`)

Praca nad ebookiem *„Jak prowadzić sprawy SKD"* (LexAlpha) wymusiła
metodologiczną korektę, która nie jest jeszcze w pełni sformalizowana w §6:
**metaanaliza v2 korpusu obaliła 3 z 15 wcześniej „potwierdzonych" tez**, bo
liczone były jako surowy odsetek rozstrzygnięć (%K), nie jako **lift ponad
bazę** (base rate). Przykład: teza „koszt-od-kosztu się załamał" (lift ±6 pp
— realnie szum) okazała się artefaktem zmiany PREWALENCJI argumentu w korpusie
(23%→57% — boilerplate, nie zmiana skuteczności).

**Propozycja: rozszerzyć R-METODA o wymóg jawnego rozróżnienia poziomu
bezwzględnego od liftu.**

> **R-METODA (rozszerzenie).** Liczba pochodząca ze zdolności statystycznej
> nie może być prezentowana jako sama wartość bezwzględna (%K), gdy koncept,
> którego dotyczy, jest **endogeniczny wobec doboru spraw** — tzn. gdy
> prewalencja argumentu w korpusie zależy od tego samego czynnika co jego
> obserwowana skuteczność (typowy przypadek: postura procesowa koreluje
> zarówno z tym, JAK CZĘSTO argument jest podnoszony, jak i z tym, jak często
> WYGRYWA). W takim wypadku konsument **powinien** obliczyć i podać **lift**
> — różnicę względem bazowego odsetka rozstrzygnięć w porównywalnej
> populacji spraw — obok albo zamiast surowego %K. Sama zmiana %K między
> dwoma oknami czasowymi bez kontroli bazy jest niediagnostyczna: może
> mierzyć zmianę siły argumentu, zmianę składu korpusu, albo oba naraz,
> i bez liftu nie da się ich rozróżnić (dowód: 3 z 15 tez metaanalizy
> korpusu SKD, uznane wcześniej za potwierdzone na surowym %K, zostały
> obalone po przeliczeniu na lift).

Dodatkowa obserwacja z tej samej metaanalizy, warta odnotowania jako
uzasadnienie dla `policzono-na-n`/`policzono-dnia` (§4.6): **sama baza
(base rate) nie jest stała w czasie** — skład korpusu wg postury przesunął
się z 1% do 46% w mierzonym okresie, więc „baza" licząca z całego korpusu
miesza populacje z różnych momentów. Lift powinien być liczony wobec bazy
**tego samego okna czasowego**, nie globalnej — inaczej powtarza ten sam błąd
jednym poziomem wyżej.

**Czego NIE proponuję dodawać.** Warstwa prezentacji (wykresy, narracja
ebooka) zostaje poza zakresem profilu (zgodnie z Nie-celami §2) — to praca
konsumenta nad wynikiem zdolności, nie treść formatu.

---

*Ten dokument uzupełnia `directed-reason.md` (§7) i proponuje jedną
konkretną zmianę do `../SPEC.md` (§6, R-METODA). Do wniesienia jako
PR/issue przy publikacji profilu.*
