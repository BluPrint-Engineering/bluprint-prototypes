> Prototype for review, not published. Short version of `README.md`. Rules come from the approved refinement (`resumo.md`, 2026-11-10); no API was designed.

# Handoff - Nova obra (#46, with #6)

Built only from BluPrint design-system primitives (Alert, Avatar, Badge, Button, Checkbox, DropdownMenu, Field, Icon, Input, Logo, SearchInput, SegmentedControl, Toast/Toaster) and the `bp-alert-dialog` classes. No new primitive, variant or glyph. The room suggestion list reuses popover tokens (`--popover`, `--shadow-md`, `--radius-lg`); in `apps/web` it is shadcn `command` inside `popover` (combobox).

| Screen | Route | What changes |
| --- | --- | --- |
| Nova obra | `/projects/new` | Nothing |
| Montar estrutura | `/projects/:id/structure/new` | Ranges and finals, Hall em cada andar unchecked with a type, entrance-hall type, two type catalogs with copy and suggestions, multi-tower rules and conflicts, discipline batches, reservation counting common areas, clickable preview with a read-only check, persistent draft |
| Acrescentar andares | `/projects/:id/structure/new?tower=` | Same ranges; Hall repeats the tower's choice and can be unchecked for this addition; reservation before saving |
| Configuração › Tipologias | `/projects/:id/configuration#tipologias` | Two groups with counts, "Sem uso", expandable rooms, copy, search; "Ver tipologias" from the structure success toast |
| Unidade / área comum | `/projects/:id/locations/:id` | Disciplines card, batch from there, room suggestions; common areas follow a common-area type |

## Who does what

| Action | Admin | Gerente | Assistente | Obra entregue |
| --- | --- | --- | --- | --- |
| Nova obra and reservation | yes | no | no | no |
| Build, add floors, types, disciplines, copy | yes | yes | yes | no |
| See preview, catalog and counts | yes | yes | yes | yes |

## Open for the technical refinement

1. Whether the draft lives in `localStorage` (prototype) or on the server, and its key (per obra and person).
2. Location disciplines are a new per-location field; RF-404 and the glossary need the amendment, and ADR 0074 the metering change (common areas spend 1).
3. The idempotent `POST /structure` must carry ranges, both catalogs, per-location disciplines and common-area types.
