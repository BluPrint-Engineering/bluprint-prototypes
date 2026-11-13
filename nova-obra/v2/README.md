> Prototype for review, not published. Parent: https://github.com/BluPrint-Engineering/bluprint-app/issues/46 (Obra) with the structure screens of https://github.com/BluPrint-Engineering/bluprint-app/issues/6 (Estrutura). Source of the rules: the approved product refinement of 2026-11-10 (`resumo.md`, Plannotator `pn-6afbd4`). Static QA files are the preview; download the interactive `*.dc.html` to run locally.

# Nova obra: ranges, two type catalogs, location disciplines, clickable preview

## Overview

The team's ten requests on #46 change how a structure is set up, not where. Nova obra (`/projects/new`) stays as shipped: admin only, name and reservation, success opens the configuration. "Montar estrutura" (`/projects/:id/structure/new`) and "Adicionar andares" (`?tower=`) gain:

- **Floor ranges** per tower instead of one "Unidades por andar": first floor, last floor and finals. A numbered floor exists only inside a range; ranges of one tower may not share a floor; a range without finals is an error. Finals fill the interval (`01, 03` gives 01, 02, 03; `A, C` gives A, B, C). Placeholder `Ex.: 01, 02` (`Ex.: A, B` in letter mode), help "Separe os finais com vírgula e espaço.".
- **Hall em cada andar** starts unchecked. Checked, each generated floor gets a Hall that spends 1 of the reservation, with one optional common-area type for all of them. "Térreo sem unidades" keeps "Com hall de entrada", starts checked, and the entrance hall has its own optional type.
- **Every created common area spends 1** of the reservation, in the wizard and when added later. It is still not a unit: no apartment number, no sale status.
- **Two type catalogs** (Unidades, Áreas comuns), created before saving, names unique in the obra ignoring case and repeated spaces, with **Copiar** (new name, rooms only, no inheritance) and **room suggestions** (fixed list plus names already used in the obra; typing filters; nothing is added until a choice, the Adicionar button or Enter).
- **Rules** apply unit types to several towers or "Todas as torres" (a snapshot: a later tower does not inherit), with "Todos os finais" or listed finals. A final a tower lacks creates nothing and is not an error. Two rules on the same unit block saving.
- **Disciplines per location.** Every new unit and common area starts with the disciplines marked as applying throughout the obra; Geral is never in the list. A batch applies one list to marked locations, a floor or a tower, in the wizard and on the saved structure. A discipline with a plan or pin on that location cannot be turned off; a batch that would do so changes nothing and names the locations.
- **Clickable preview.** Each preview cell is a button that opens a read-only check (path, type, rooms, disciplines, reservation) with "Editar na etapa" links to the step that owns each field. Desktop: a panel beside the preview. Mobile: it covers the preview; closing returns to the same scroll position.
- **Draft** survives a reload until it is created or discarded; leaving asks "Descartar o que você montou?"; a failed save keeps it and retries without creating twice.
- **Configuração › Tipologias**: two groups with counts, expandable rooms, "Sem uso" badge, copy, search when the list is long; the structure success toast links to it ("Ver tipologias").

## Files

| File | What it is |
| --- | --- |
| `Nova obra.dc.html` | Live screens. Demo props (also from the URL): `screen` (new / wizard / extend / home / tower / floor / unit / area / configuration), `state` (a frozen state id below or `live`), `role` (admin / manager / assistant), `theme`, `viewport`, `delivered`, `createResult` (success / fail / no-units), `unitsLeft` |
| `Nova obra QA.dc.html` | Every state below frozen, mobile 390 × 844 and desktop 1280 × 800, light and dark |
| `Nova obra QA-{N,W,A,C,L}-static.html` | Script-free snapshots of those states, grouped by prefix, with state anchors |

## States

The handoff's eleven states per changed screen, mapped. "n/a" names states the screen cannot reach.

| Screen | Empty | Filled | Invalid | Reservation short | Saving | Failure + retry | Draft restored | Rule conflict | Discipline conflict | Delivered | Role without permission |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Montar estrutura (W) | W1 | W2, W2b, W3–W8b | W9 | W10 (manager), W10b (admin) | W11 | W12 | W13 | W5b | n/a: nothing has content before saving | W15 | W10: only the admin changes the reservation |
| Acrescentar andares (A) | A1 | A2, A3 | A4 | A5 (assistant) | A6 | A7 | same draft as W13 | same rules as W5b | n/a | A9 | A5 |
| Configuração › Tipologias (C) | C8 | C1, C2, C5 | C3b, C4b | n/a: types spend nothing | C6 | C7 | n/a | n/a | n/a | C9 | n/a: every role edits types; C3 is the assistant |
| Local (L) | L4 | L1, L2, L7 | L5 (suggestion) | L8b | n/a: toggles save at once | L9 | n/a | n/a | L3, L6b | L10 | n/a: every role edits disciplines |
| Nova obra (N) | N1 | N2 | as #46 N3a–N3d | as #46 N5–N6b | as #46 N4 | as #46 N7 | n/a | n/a | n/a | n/a | not reachable: gerente and assistente are redirected (#46) |

| Id | State |
| --- | --- |
| N1–N2 | Nova obra empty and filled, unchanged from #46 |
| W1 | Fresh wizard: one empty range, Hall em cada andar unchecked |
| W2 | The team's request: Torre A 2º–10º `01, 02`, 11º–17º `01` (25 apartments); Torre B `01, 04` fills 01–04 |
| W2b | Letter numbering, `Ex.: A, B` |
| W3 | Common areas with an optional type, each spends 1 |
| W4, W4b, W4d, W4c | Two catalogs with rooms as one-per-row lists; suggestion `sa` grouped as Sugestões (Sala, Sacada) and Usados nesta obra (Salão); rooms already in the type shown checked and disabled; copy dialog |
| W5, W5b, W5c, W5d | Rules pick towers, finals and floors from menus of what exists (no typing); rule conflict names the rules, units, final and floors and blocks saving; finals menu; towers menu with "Uma torre criada depois não entra nesta regra." |
| W6, W6b | The obra discipline list (Hidráulica and Elétrica default, others created in the step or the batch); batch; new discipline name taken |
| W7, W7b | Preview with "Esta estrutura usa 81 de 400 reservadas."; marking locations |
| W8, W8b | Check panel for 201 and for a Hall |
| W9 | Overlapping ranges and a range without finals |
| W10, W10b | Reservation short, manager and admin |
| W11–W16 | Saving, failure with retry, draft restored, leaving with a draft, delivered, structure created with "Ver tipologias" |
| A1–A9 | Add floors: empty, filled with Hall repeated, Hall unchecked for this addition, crossing existing floors, reservation short, saving, failure, added, delivered |
| C1–C9 | Tipologias: two groups (Sem uso visible), rooms open, copy, copy name taken, editing with suggestion, duplicate room, search, saving, failure, empty, delivered |
| L1–L10 | Unit 201 disciplines, 201 without Pintura, conflict (Hidráulica has pins), no specific discipline, room suggestion on Garagem, batch from the floor, batch conflict, Hall with its type, adding a common area (spends 1), reservation over, failure, delivered |

## Copy added beyond the approved handoff

The approved copy is used verbatim. These strings were needed and are proposals:

- Reservation short: the approved sentence plus "Faltam 51." and the shipped way out ("Alterar reserva" for the admin, "Fale com o admin da construtora." otherwise).
- Reservation line: the approved sentence plus "Depois de criar, sobram 319." and the breakdown "61 unidades + 16 halls de andar + 2 halls de entrada + 2 outras áreas comuns".
- Range errors: "Informe os finais desta faixa.", "Use números de 1 a 30, como 01, 02." / "Use letras, como A, B.", "Esta faixa repete o 10º andar da faixa 1. Cada andar fica numa faixa só.", "A Torre A já tem do 2º ao 17º andar. Use andares que ela ainda não tem."
- Rules (round 2, replaces the approved conflict sentence at the owner's request): alert title "Duas regras dão tipologia às mesmas unidades", body "Regra 2 (Tipologia B) e regra 4 (Tipologia A): 202 a 1002, na Torre B." and "Cada unidade fica com uma tipologia só. Tire o final ou os andares repetidos de uma das regras para salvar."; on the card "O final 02 da Torre B, do 2º ao 10º andar, já tem a Tipologia A pela regra 4."; menu note "Uma torre criada depois não entra nesta regra."; "Escolha ao menos um final."
- Check panel links: "Mudar na faixa 1 da Torre A", "Mudar na regra 1", "Mudar na Tipologia A", "Mudar em Disciplinas" (mobile adds "· passo N").
- Disciplines: "Nova disciplina", "Padrão", "Já existe a disciplina Elétrica nesta obra.", "Depois de criar, ligue ou desligue disciplinas em cada local ou em lote. A escolha de onde uma disciplina vale fica definida na criação. Cor e planta própria se ajustam na configuração."
- Disciplines: "Geral vale em todo local e não entra na lista. Pin novo aqui oferece …", "Nenhuma disciplina específica ligada. Pin novo aqui, nem na Geral, só depois de ligar uma.", "Nada mudou." over the per-location sentences.
- Draft: "Rascunho restaurado" / "O que você montou continua aqui até criar a estrutura ou descartar."; leave dialog body "Nada foi salvo ainda. As torres, as tipologias e as regras deste rascunho são apagadas." with "Continuar montando" / "Descartar".
- Copy and new type: "Nome da cópia", "Único na obra, entre as tipologias de unidade e de área comum.", "Já existe uma tipologia com esse nome nesta obra."
- Room in a location: "Esse cômodo já está na unidade." / "na área comum."

## Round 3 copy (owner asked to revise unapproved errors)

- Reservation short: title "Faltam 51 unidades na reserva", body "Esta estrutura usa 81 e a obra ainda tem 30.", then "Peça mais ao admin da construtora." or the admin's "Alterar reserva".
- Save failure: title "Não foi possível criar a estrutura" / "Não foi possível adicionar os andares", body "Nada foi salvo. Confira a conexão.", button "Tentar de novo".
- Ranges: "O 10º andar já está na faixa 1. Cada andar fica numa faixa só.", "A Torre A já vai até o 17º andar. Comece no 18º."; incomplete ranges in the preview: "Complete as faixas desta torre para ver os andares." (neutral).
- Names: "Esse nome já existe nesta obra.", "Elétrica já existe nesta obra."
- Disciplines: "Hidráulica tem planta ou pin na 201, então continua ligada."; batch "Nada mudou" + "201: Hidráulica tem planta ou pin." + "Deixe essas disciplinas marcadas ou tire esses locais do lote."
- Type save failure: title "Não foi possível salvar", body "A Tipologia B não mudou. Confira a conexão e tente de novo."; discipline toggle failure toast "As disciplinas da 201 não mudaram. Confira a conexão e tente de novo."

## Layout decisions

- Desktop header scrolls away with the page (round 3), so the sticky preview fits the viewport. #46 now says the same: the app header scrolls on desktop and phone.
- The desktop check replaces the preview in its column; mobile covers the screen. Both return to the same position.
- Rooms are always one-per-row lists (16px, 44px rows), read-only or editable, on mobile and desktop (round 4: the grid was rejected).

- Ranges are small cards inside the tower card, three fields in a row on desktop, finals full width on mobile; each shows its total and the filled finals ("36 unidades · 9 andares · finais 01, 02, 03 e 04"), so the fill is visible before saving.
- Rules became cards with menus for towers and finals and selects for floors, offering only what the towers have, so a rule cannot name a final or floor that does not exist.
- Type rooms are one-per-row lists, not pills.
- The desktop preview is sticky at full height; only its façade list scrolls, except in a short window, when the whole preview scrolls once the façade list would fall under 180px. The desktop check panel opens beside it and dims the form.
- Preview cells are 44px on mobile (were 34px) because they are now tappable; 26px on desktop (were 22px).
- The check panel floats left of the sticky preview on desktop (shadow, ring) and covers the screen on mobile with a "Prévia" back link.
- The structure success toast carries "Ver tipologias" as a link under the text, so it never squeezes the message on a phone.

## Prototype limits

- After "Criar estrutura" the façade shows the demo obra (Torre A as requested, Torre B), not the typed draft.
- Menu items unrelated to this change (rename, move, duplicate, remove) open nothing; they behave as in #6.
- "Trocar tipologia" and "Alterar reserva" are visible and inert.


## Local foundation addendum

Foundation decisions 9-17 supersede the original discipline defaults. Floor/tower lists are independent and start with Hidráulica and Elétrica. New units/common areas inherit whole-obra disciplines only. The creation scope is immutable. Geral stays outside every list. Existing fixture units retain their explicit enabled lists. Impermeabilização is the selective example; Pintura applies throughout the obra.

The QA build contains only new/changed W and L states for this review. Original N/A/C and other W states remain in the interactive prototype and source inventory.

New states:
- W6c: Nova disciplina · escolha pendente. Dois campos: nome e onde se aplica. Criar só após escolher.
- W6d: Nova disciplina · na obra toda. Ligada nas unidades e áreas comuns existentes e novas. Andares e torres mantêm suas listas.
- W6e: Nova disciplina · só onde for ligada. Nasce desligada. A escolha fica definida na criação.
- W6f: Catálogo e ajustes. Pintura na obra toda; Impermeabilização só onde for ligada. Andar e torre começam com as duas básicas.
- L11: Disciplinas do andar. Lista própria: Hidráulica e Elétrica. A 201 continua com suas quatro disciplinas.
- L11b: Andar sem disciplina específica. As unidades e áreas comuns continuam com suas disciplinas.
- L11c: Andar com planta. A trava vale para conteúdo do próprio andar.
- L12: Disciplinas da torre. Lista própria: Hidráulica e Elétrica. Independente dos andares, unidades e áreas comuns.
- L13: Nova disciplina no Local. Nome e onde se aplica. Sem cor nem planta própria aqui.
- L13b: Local · na obra toda. Gesso ligada nas unidades e áreas comuns existentes e novas; torres e andares não mudam.
- L13c: Local · só onde for ligada. Piscina desligada até ligar em cada local ou em lote.
- L13d: Local · nome em uso. Mantém nome e escolha ao recusar a duplicata.
- L14: Unidade nova · 203. Começa com Hidráulica, Elétrica e Pintura. Impermeabilização, só onde for ligada, começa desligada.
- L15: Andar depois do lote. 201, 202 e Hall só com Elétrica. O próprio andar continua com Hidráulica e Elétrica.
- L16: Andar · obra entregue. Lista só para ler, sem criar ou aplicar.
- L17: Torre · obra entregue. Lista só para ler, sem criar ou aplicar.

Changed states: W6, W6b (creation entry and scope), W8/W8b (new-location defaults); L1-L5, L7, L9 (Nova disciplina entry), L6/L6b (scope-aware creation and independence copy), L8/L8b (floor discipline panel), L10 (unchanged read-only controls, retained for regression comparison).

Creation copy: Onde se aplica; Na obra toda; Só onde for ligada; Essa escolha não muda depois. No scope is preselected. Cancel returns to a pending batch without losing its choices. Batch creation selects the new discipline in that batch; applying remains explicit.
