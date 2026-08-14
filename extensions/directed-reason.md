# Racja kierunkowa jako jednostka wyszukiwania
## Propozycja sekcji do OKF-Legal (rozszerzenie §3.1)

*Draft do dyskusji środowiskowej. Sierpień 2026.*

---

## 1. Czego nie wymyśliliśmy

Uczciwe zakotwiczenie musi być pierwsze, bo bez niego cała reszta wygląda na
przypisywanie sobie cudzego pomysłu.

**Myśl, że jednostką wyszukiwania powinno być twierdzenie, a nie fragment tekstu,
istnieje w NLP i jest opisana.** Chen i in. w pracy *Dense X Retrieval: What
Retrieval Granularity Should We Use?* wprowadzili **propozycję** — atomowe,
samodzielne wyrażenie niosące jeden fakt — jako jednostkę indeksowania, i wykazali
przewagę nad zdaniami i akapitami w retrievalu dziedzinowym. Pokrewna linia to
atomowe fakty w weryfikacji twierdzeń i dekompozycja twierdzeń w ocenie
faktograficzności.

Nasz wkład nie polega więc na odkryciu, że mniejsza jednostka wyszukuje się
lepiej. **Racja kierunkowa jest rozwinięciem propozycji Chena i in. o warstwy,
których tekst prawniczy wymaga, a tekst encyklopedyczny nie.** Propozycja
sprawdza się tam, gdzie zdanie jest faktem. W uzasadnieniu wyroku zdanie faktem
zwykle nie jest — jest stanowiskiem w sporze, a stanowisko bez wskazania, czyje
i jak rozstrzygnięte, nie nadaje się do użycia w piśmie procesowym.

---

## 2. Dlaczego propozycja to za mało w prawie

Propozycja jest z definicji **bezkierunkowa**. „Prowizja wynosiła 25% kapitału"
to fakt. Ale zdanie z uzasadnienia wyroku prawie nigdy nie jest samym faktem —
jest **racją w sporze**, wypowiedzianą przez kogoś, na czyjąś korzyść, w określonym
trybie i miejscu wywodu.

Zapytanie prawnika brzmi „podaj racje przeciwko tezie X", a nie „podaj zdania
o X". Jeśli jednostka wyszukiwania nie niesie kierunku, wynik zapytania miesza
argumenty obu stron i jest w praktyce mylący — bo pełnomocnik cytujący w piśmie
zdanie z wyroku, w którym sąd referował stanowisko przeciwnika, popełnia błąd
warsztatowy o poważnych konsekwencjach.

**Racja kierunkowa** (ang. *directed reason*) — twierdzenie wyprowadzone z tekstu
prawniczego, zakotwiczone w cytacie verbatim, opatrzone informacją, na czyją
korzyść zostało rozstrzygnięte oraz gdzie leży w toku wywodu.

Formalnie: **racja kierunkowa = propozycja + cztery warstwy**, których w ujęciu
ogólnym nie ma:

| Warstwa | Co niesie | Czemu bez niej racja jest bezużyteczna |
|---|---|---|
| **Kierunek** | na czyją korzyść rozstrzygnięto tę rację | bez tego „racje za" i „racje przeciw" wracają wymieszane |
| **Tryb** | ratio czy obiter, racja własna sądu czy referowana | cytowanie obiter dictum jako podstawy rozstrzygnięcia to błąd |
| **Instancja źródłowa** | czy to rozumowanie tego sądu, czy przejęte od sądu niższego | inaczej statystyka liczy przegraną jako wygraną |
| **Pozycja** | miejsce w wywodzie dokumentu-rodzica | pozwala odtworzyć **kolejność** rozumowania, nie tylko jego składniki |

Do tego dochodzi **zakotwiczenie**: racja bez cytatu verbatim ze źródła jest
twierdzeniem modelu, a nie faktem z wyroku. To warunek konieczny użycia
procesowego i jedyna warstwa, która czyni całość audytowalną.

---

## 3. Co z tego wynika, a czego propozycja nie daje

Dwie konsekwencje są nieoczywiste i to one uzasadniają osobne pojęcie.

**Pozycja zamienia zbiór cytatów w sekwencję.** Rozumowanie sądu to nie worek
argumentów, tylko ich porządek: charakterystyka instytucji, cel, wykładnia,
subsumpcja, decyzja. Ponieważ każda racja ma kotwicę lokalizowaną w tekście,
porządek jest odtwarzalny **deterministycznie** — bez pytania modelu „jak sąd
rozumował". Rekonstrukcja wywodu przestaje być generacją, a staje się sortowaniem.

**Kierunek zamienia wyszukiwarkę treści w wyszukiwarkę rozstrzygnięć.** Dopiero
gdy racja niesie kierunek, można ją agregować z innymi z wielu wyroków
w **oś sporną** — zagadnienie, w którym orzecznictwo dzieli się na dwa obozy,
z rozkładem i przedstawicielami po obu stronach. Oś sporna jest bytem, którego
nie da się zbudować ani z dokumentów, ani z chunków, ani z bezkierunkowych propozycji.

---

## 4. Dowód empiryczny

**Trafność retrievalu.** W ślepym teście (sędzia-model nieświadomy toru, 30 pytań)
wyszukiwanie po racjach kierunkowych dawało odpowiedź wprost na **75%** pytań praktycznych
i **80%** pytań brzegowych typu „podaj racje sądów za tezą X". Klasyczne chunki
dokumentu — odpowiednio **45%** i **30%**. Różnica rośnie wraz z trudnością
pytania, co jest zgodne z intuicją: chunk opowiada okolicę tematu, racja
odpowiada na pytanie.

**Ślepa fuzja szkodzi.** Połączenie obu torów metodą RRF na krótkiej liście
wyników **rozcieńcza racje** (65% i 70% zamiast 75% i 80%). Wniosek wdrożeniowy:
racje prowadzą, chunki służą jako okno kontekstu wokół trafionej racji, a szeroka
fuzja ma sens tylko przed rerankingiem.

**Skala.** Poziom racji utrzymany produkcyjnie: **135 090 racji kierunkowych wyprowadzonych
z 1095 wyroków**, przy 95% cytatów zgodnych co do znaku ze źródłem (mediana 97%
na wyrok). To nie jest demonstrator — to jest korpus, na którym pracuje kancelaria.

**Rzecz, której chunk nie umie w ogóle.** Z racji kierunkowych zbudowaliśmy rejestr **490 osi
spornych** z rozkładem rozstrzygnięć po obu stronach. Zapytanie „na czym dokładnie
dzieli się orzecznictwo w tym zagadnieniu i kto stoi po której stronie" jest
w modelu chunkowym niewykonalne, bo wymaga jednostki, która niesie kierunek.

---

## 5. Czynnik X — to, co opiera się matematyce i kategoryzacji

Poprzednia sekcja pokazuje, że racje kierunkowe wyszukują się lepiej. To jednak
argument techniczny i nie o niego tu ostatecznie chodzi. Głębszy powód jest taki,
że **w rozstrzygnięciu prawniczym zostaje reszta, której nie da się ani policzyć,
ani zaszufladkować** — a jednostka danych albo tę resztę przenosi, albo ją niszczy.

### 5.1 Reszta jest mierzalna w swoim istnieniu, choć nie w swojej treści

Nie jest to teza filozoficzna. Zmierzyliśmy ją własnymi narzędziami, i to trzy razy,
za każdym razem wbrew własnym oczekiwaniom.

**Statystyka argumentów rozpada się po kontroli.** Kiedy przeszliśmy z surowego
odsetka rozstrzygnięć na przyrost ponad porównywalne sprawy, **większość różnic
między typami zarzutów zmieściła się w granicach kilku punktów procentowych,
czyli w szumie**. Na 490 osi spornych realną informację niosły dwie. Reszta
rankingów, które w tej dziedzinie krążą, mierzy dobór spraw, a nie siłę argumentu.

**Kategoryzacja gubi to, co decyduje.** Próba zbudowania „archetypu wyroku
prokonsumenckiego" z etykiet skończyła się tautologią: kwalifikator „racja
decydująca" okazał się pusty, bo 97–100% obecnych zagadnień ma rację decydującą.
Klasa nie odróżnia spraw, bo różnica leży wewnątrz klasy.

**Skuteczność jest zlokalizowana, nie ogólna.** Największe odchylenie w całym
korpusie okazało się po rozbiorze nie własnością sądu, lecz jednego wydziału,
po zwrocie linii w konkretnym roku, realizowanym przez trzech sędziów, z czego
jeden orzekał w dwóch trzecich tych spraw. To, co statystyka podała jako
„efekt sądu", było w istocie zdarzeniem osadzonym w składzie i w czasie.

Wniosek nie brzmi „statystyka jest bezużyteczna". Brzmi: **statystyka wyznacza
granice tego, co daje się uogólnić, a poza tą granicą leży czynnik X** — dopasowanie
konkretnego sformułowania do konkretnej ramy myślowej konkretnego składu w konkretnym
momencie linii orzeczniczej.

### 5.2 Dlaczego chunk i tabela niszczą X, każde na swój sposób

**Chunk niszczy X przez rozmycie.** Fragment akapitu niesie okolicę tematu,
ale gubi to, czyją rację wyraża i w którym ogniwie wywodu leży. Zwraca podobieństwo
tematyczne, a X jest w różnicy, nie w podobieństwie.

**Kategoria niszczy X przez spłaszczenie.** Etykieta „zarzut informacyjny" łączy
w jedno uchybienie techniczne i brak informacji, który realnie uniemożliwił ocenę
zobowiązania. Po przypisaniu do klasy różnica przestaje istnieć w danych, choć
w orzeczeniach decyduje o wyniku.

**Liczba niszczy X przez agregację.** Odsetek to operacja, która z definicji
usuwa pojedynczy przypadek. A w prawie pojedynczy przypadek bywa całą wartością:
jedno zdanie sądu, sformułowane w określony sposób, jest tym, co da się przenieść
do pisma.

### 5.3 Racja kierunkowa jest jednostką, która X przenosi

Cztery warstwy z §2 nie są ozdobnikiem — każda chroni inny wymiar reszty.

**Cytat verbatim** zachowuje sformułowanie zamiast jego streszczenia. To jest
nośnik X w najczystszej postaci: parafraza opisuje, o czym sąd mówił, cytat
zachowuje **jak** to powiedział, a w argumentacji prawniczej różnica między tym
dwojgiem bywa różnicą między wygraną a przegraną.

**Kierunek** zachowuje spór. Bez niego zostaje temat, a X mieszka w tym, że dwa
sądy o tym samym temacie orzekły przeciwnie i warto wiedzieć, którymi słowami.

**Pozycja** zachowuje tok. Kolejność ogniw jest częścią argumentu, a nie jego
metadaną; ta sama racja postawiona jako pierwsza i jako czwarta znaczy w wywodzie
co innego.

**Tryb i instancja źródłowa** zachowują status wypowiedzi — czy to podstawa
rozstrzygnięcia, uwaga na marginesie, czy cudze rozumowanie przejęte przez sąd
wyższy.

Innymi słowy: **racja kierunkowa jest zaprojektowana tak, żeby agregat dało się
z niej wyprowadzić, ale żeby nigdy jej nie zastąpił.** Statystyka osi spornej jest
widokiem nad racjami, a nie bytem samodzielnym; z każdej liczby da się zejść
z powrotem do cytatów, które ją utworzyły, i zobaczyć, czym te sprawy różniły się
poza kategorią.

### 5.4 Konsekwencja dla profilu

Z tego wynika reguła mocniejsza niż zalecenie techniczne:

> **Warstwa agregatów nie może być publikowana bez warstwy racji, z których
> powstała.** Bundle zawierający wyłącznie rozkłady i statystyki osi jest
> w rozumieniu profilu niekompletny, ponieważ odbiera konsumentowi możliwość
> zejścia do materiału, w którym leży czynnik X.

To jest zarazem test na wartość całego pomysłu. Jeżeli reszta nie istnieje,
wystarczą tabele i profil jest przerostem formy. Nasze trzy pomiary mówią, że
istnieje i że jest duża — dlatego jednostką bazową musi być coś, co ją przenosi.

### 5.5 Uczciwe ograniczenie: X to nie mistyka

Czynnik X nie jest wezwaniem do rezygnacji z pomiaru ani zasłoną dla braku metody.
Jest resztą po odjęciu tego, co wyjaśnione, i dzieli się co najmniej na dwie części,
które trzeba rozróżniać.

Część pierwsza to **reszta wywodu**: realna nieprzewidywalność rozstrzygnięcia,
wynikająca z indywidualnego dopasowania argumentu do sprawy i do składu.

Część druga to **reszta pomiaru**: nasze własne błędy — grubość kategorii, dryf
etykietowania, ograniczenia korpusu. Ta część jest redukowalna i redukowanie jej
jest obowiązkiem, a nie ambicją.

Pomieszanie obu jest najczęstszym nadużyciem w tej dziedzinie: własna nieudolność
pomiarowa bywa sprzedawana jako tajemnica prawa, a rzeczywista nieprzewidywalność
jako defekt do naprawienia większym modelem. Profil powinien wymagać, by konsument
deklarował, którą część opisuje.

---

## 6. Granice — co ta teza kosztuje

Sekcja nie byłaby uczciwa bez tego akapitu.

**Racje kierunkowe nie powstają za darmo.** Wymagają ekstrakcji z kontrolą dosłowności, a ta
kosztuje. W naszym przypadku rząd wielkości to złotówka za wyrok przy hybrydowym
doborze modeli, po wcześniejszym zejściu z sześciu.

**Poziom racji wymaga mapy sekcji dokumentu-rodzica.** Wyroki zassane bez
segmentacji uzasadnienia okazały się dla ekstraktora niewidoczne — nie dało się
ich rozpisać w ogóle. To argument za regułą konformancji: bundle deklarujący
poziom racji musi nieść mapę sekcji z kotwicami.

**Reguła wyprowadzenia rozstrzygnięcia osi musi być deklarowana.** Statystyki
liczone z racji zależą od tego, jak zdefiniuje się „oś rozstrzygniętą na korzyść".
Dwa zgodne z profilem bundle, przy różnych regułach agregacji, dadzą sprzeczne
liczby — sprawdziliśmy to na własnych danych i różnice sięgały kilkudziesięciu
punktów procentowych.

**Udział rozstrzygnięć na osi nie mierzy siły argumentu.** Mierzy przede wszystkim
dobór spraw, w których dany argument bywa podnoszony. Konsument profilu powinien
być przed tym ostrzeżony wprost, bo to najczęstsza pułapka w analizie orzecznictwa.

---

## 7. Propozycja do dyskusji

Do rozstrzygnięcia w środowisku pozostaje, czy warstwa kierunku powinna być
obowiązkowa dla wszystkich racji, czy tylko dla tych wywodzonych z części
orzeczniczej uzasadnienia, oraz jak nazwać jednostki, które kierunku nie niosą
(tezy ogólne, przesłanki testów) — czy pozostają racjami kierunkowymi z pustym
polem, czy stanowią osobny typ.

Twierdzenie, które zgłaszamy do dyskusji, brzmi tak: **w tekście prawniczym
jednostką wyszukiwania nie jest fragment ani nawet twierdzenie, lecz twierdzenie
z kierunkiem i pozycją w wywodzie** — ponieważ to, co w rozstrzygnięciu decyduje,
w znacznej części opiera się matematyce i kategoryzacji, a jednostka danych albo
tę resztę przenosi, albo ją bezpowrotnie usuwa. Reszta profilu jest konsekwencją
tej jednej decyzji.

---

### Źródła zewnętrzne

- Chen i in., *Dense X Retrieval: What Retrieval Granularity Should We Use?* —
  propozycja jako jednostka retrievalu, arXiv:2312.06648.
- Min i in., *FActScore: Fine-grained Atomic Evaluation of Factual Precision
  in Long Form Text Generation* (2023) — dekompozycja na atomowe fakty
  w weryfikacji faktograficznej; sąsiednia inspiracja, fakt bezkierunkowy
  z definicji (linia kontynuowana: VeriFastScore, DnDScore, OpenFActScore).
- Bhattacharya i in., *Identification of Rhetorical Roles of Sentences in
  Indian Legal Judgments* (2019, arXiv:1911.05405) i rozwinięcia: Malik i in.
  (13 ról, multi-task learning), Bambroo i in. *MARRO* (2025,
  arXiv:2503.10659), *LegalSeg* (2025, arXiv:2502.05836), *Segment First,
  Retrieve Better* (2025, arXiv:2508.00679) — najbliższy istniejący nurt do
  warstwy „tryb"; rola retoryczna bez kierunku i bez agregacji w rozkład.
- Poudyal i in., *ECHR: Legal Corpus for Argument Mining* (ACL 2020,
  argmining-1.8) i rozwinięcia (373 orzeczenia, Springer AI & Law 2023) —
  struktura premise/conclusion bez statusu procesowego (ratio/obiter).
- Shepard's Citations / Westlaw KeyCite (`followed`/`distinguished`/
  `overruled`) i świeży benchmark LLM *Validate Your Authority* (2026,
  arXiv:2605.17691) — jedyna istniejąca realizacja warstwy „kierunek", ale
  wyłącznie na poziomie całego cytowania orzeczenie→orzeczenie, nigdy
  pojedynczego argumentu, i bez agregacji w rozkład typu oś sporna.
- SAILER (arXiv:2304.11370) i ReaKase (arXiv:2510.26178) — struktura
  dokumentu jako sygnał dopasowania case-do-case, nie ekstrakcji atomów.
- Lex Machina / Trellis — analityka sporów na metadanych sprawy
  (sędzia/typ wniosku/kancelaria), bez jednostki „racja" w ogóle.

Pełny przegląd z cytatami i tabelą porównawczą: `prior-art-and-lift-metric.md`.
Żadna ze sprawdzonych prac nie łączy kierunku + trybu + instancji źródłowej +
pozycji + cytatu w jednej jednostce agregowalnej w policzalną oś sporną —
wynik przeglądu wzmacnia, nie osłabia, tezę tej sekcji.
