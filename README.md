# OKF-Legal

**Profil prawniczy nad [Open Knowledge Format (OKF)](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf)** —
propozycja środowiskowa, nie norma wydana przez jakikolwiek podmiot.

Powstał w naszej kancelarii (**Kancelaria Radców Prawnych Żurawska Piotrowski
i Wspólnicy**, [ktzr.pl](https://ktzr.pl)), jako destylacja struktur, do
których niezależnie doszły dwa działające systemy wiedzy prawniczej zbudowane
przez tych samych autorów — baza orzecznictwa i skill do redakcji umów
([commercial-legal-pl](https://github.com/apiotrowski-afk/commercial-legal-pl)).

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Status](https://img.shields.io/badge/Status-1.0_(pierwsza_wersja_publiczna)-green)](https://github.com/apiotrowski-afk/okf-legal)
[![OKF](https://img.shields.io/badge/OKF-v0.2-orange)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)

> ⚠️ Ten profil nie jest źródłem prawa ani nie zastępuje weryfikacji przez
> uprawnioną osobę. To warstwa reprezentacji *wiedzy o prawie*, nie publikator
> aktów.

## English summary

**OKF-Legal** is a legal-domain profile over Google's [Open Knowledge Format
(OKF)](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf) —
a proposal for representing legal knowledge (case law, statutes, contract
clauses) as markdown files an LLM can consume directly, without a
vendor-specific pipeline.

Its central idea is the **directed reason**: a sub-document unit for a single
argument or holding, carrying a verbatim source quote, its direction (which
side it favors), its procedural weight (*ratio decidendi* vs. *obiter
dictum*), the originating court instance, and its position in the court's
reasoning. In a blind retrieval test (30 questions, a model judge unaware of
which method produced each answer), retrieval over these units answered
"directly" 75–80% of questions, vs. 30–45% for standard document chunks.

The profile also specifies hard anti-hallucination consumption rules: a quote
must be locatable verbatim in its source, a statistic must carry its sample
size and computation date, and a nearest-neighbor search result is never
treated as evidence that a matching precedent actually exists.

Built from two independent production systems, a Polish case-law corpus
(~1,100 judgments, ~135k reasoning atoms) and an open-source contract-drafting
skill, [commercial-legal-pl](https://github.com/apiotrowski-afk/commercial-legal-pl),
that converged on the same structures without a shared design. Apache 2.0.

**Full read in English:** [`SPEC.en.md`](./SPEC.en.md) (the condensed
specification — type vocabulary, frontmatter fields, consumption rules,
empirical evidence table) and
[`extensions/directed-reason.en.md`](./extensions/directed-reason.en.md)
(the argument for why a directed reason, not a document or a chunk, has to
be the retrieval unit — prior art, evidence, and the "factor X" section on
what statistics and categories structurally destroy). The Polish originals
(`SPEC.md`, `extensions/directed-reason.md`) are normative; the English
versions are condensed and section-numbered to match them 1:1.

## Po co to publikujemy

OKF opisuje, jak reprezentować wiedzę jako przenośne pliki markdown, które
model językowy może czytać bez integracji specyficznej dla narzędzia. Rdzeń
jest celowo generyczny — nie mówi nic o tym, czego dodatkowo wymaga praca
prawnicza: jednostki wiedzy mniejszej niż dokument, kierunku rozstrzygnięcia,
rozróżnienia ratio/obiter, ani reguł konsumpcji chroniących przed
halucynacją tam, gdzie koszt błędu jest zawodowy.

Ten profil nie jest wyprowadzony z pierwszych zasad. Jest **destylacją tego,
do czego niezależnie doszły dwa działające systemy** — baza ~1100 wyroków
z ~135 tys. atomami rozumowania (orzecznictwo Sankcji Kredytu Darmowego) oraz
publiczny skill do redakcji umów B2B/IP/IT. Oba, mimo skrajnie różnych
nośników (baza danych z embeddingami vs pliki markdown czytane przez model),
wykształciły te same struktury bez wspólnego projektu z góry. Pełne
uzasadnienie i dowody empiryczne — w `SPEC.md`.

Publikujemy to z tego samego powodu co `commercial-legal-pl`: nie po to, żeby
ogłosić gotowy standard, tylko żeby pokazać jedną konkretną propozycję,
otwartą na krytykę, fork i dyskusję — zwłaszcza że ekosystem legal-AI wokół
LLM-wiki/OKF/RAG dziś prawie w całości mówi o common law i języku angielskim.

## Co w środku

| Plik | Co zawiera |
|---|---|
| [`SPEC.md`](./SPEC.md) | Główna specyfikacja (normatywna, PL): słownik typów konceptów, jednostka pod-dokumentowa (atom), rozszerzenia frontmattera, relacje typowane z kierunkiem, reguły konsumpcji anty-halucynacyjne, zdolności analityczne, konformancja. |
| [`SPEC.en.md`](./SPEC.en.md) | Skrócona wersja `SPEC.md` po angielsku, numeracja sekcji 1:1 ze spec-em PL. |
| [`extensions/directed-reason.md`](./extensions/directed-reason.md) | Rozwinięcie §3.1 — **racja kierunkowa** (directed reason) jako jednostka retrievalu: zakotwiczenie wobec Chen i in. (*Dense X Retrieval*), formalna definicja, dowód empiryczny, i sekcja o granicach tego, co daje się policzyć i skategoryzować w prawie. |
| [`extensions/directed-reason.en.md`](./extensions/directed-reason.en.md) | Wersja powyższego po angielsku. |
| [`extensions/prior-art-and-lift-metric.md`](./extensions/prior-art-and-lift-metric.md) | Przegląd prior art w legal NLP (rhetorical role labeling, argument mining, precedent treatment classification) + propozycja rozszerzenia reguły R-METODA o lift ponad bazę zamiast surowego odsetka, z doświadczenia publikacji popularyzacyjnej opartej na korpusie. |

## Status i jak dołączyć

To pierwsza wersja publiczna (1.0). Wszystkie decyzje projektowe pozostają
otwarte — najważniejsze zebrane w Załączniku B `SPEC.md`. Chętnie przyjmiemy:

- **Issues** — pytania, kontrprzykłady, przypadki, których profil nie
  obsługuje.
- **Pull requesty** — poprawki, rozszerzenia słowników (§3/§5), nowe
  zdolności (§4.6), inne jurysdykcje.
- **Fork** — jeśli Twoja praktyka wymaga innych decyzji projektowych, forkuj
  i dostosuj. Profil nie ma właściciela; wartość zależy od tego, ilu
  producentów i konsumentów potrafi się nim posłużyć, nie od tego, kto go
  opublikował.

## Licencja

[Apache License 2.0](./LICENSE).
