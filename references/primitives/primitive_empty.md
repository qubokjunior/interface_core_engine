# Primitive: Empty

Status: concept reference → schema-ready reference  
Canonical id: `primitive.empty`  
Discriminant field: `primitive_type: "primitive.empty"`  
Schema version: `1`

`primitive.empty` to niewidoczny lub debugowo widoczny element bazowy. Służy jako placeholder, kotwica, marker snapowania, punkt debugowy, future slot albo neutralny obiekt struktury projektu. Nie jest widzialnym kształtem i nie powinien przejmować odpowiedzialności `primitive.rectangle` ani `primitive.region`.

## Core intent

| aspekt | decyzja |
|---|---|
| Główna rola | Lekki obiekt referencyjny: placeholder / anchor / marker / future slot. |
| Render normalny | Domyślnie brak renderingu. |
| Render debug | Cross, dot, bounds, ghost rectangle albo label. |
| Layout | Opcjonalny udział w layout; domyślnie nie zajmuje miejsca. |
| Hit-test | Domyślnie wyłączony. Może działać jako anchor/debug target. |
| Dzieci | Domyślnie nie przyjmuje dzieci. Jeśli zaczyna działać jak obszar, należy użyć `primitive.region`. |

## Parametry

| grupa | parametr | typ wartości | domyślnie | wymagane | zapis / runtime | opis / reguła |
|---|---|---:|---:|---:|---|---|
| base | schema_version | integer | 1 | yes | save | Wersja schematu do migracji danych. |
| identity | id | id_ref | auto | yes | save | Unikalny identyfikator instancji. |
| identity | name | string | `Empty` | yes | save | Nazwa w outlinerze/inspektorze. |
| identity | primitive_type | enum | `primitive.empty` | yes | save | Stały typ prymitywu. |
| identity | description | long_text | empty | no | save | Opis techniczny lub projektowy. |
| identity | notes | long_text | empty | no | save | Notatka użytkownika, osobna od `metadata`. |
| identity | tags | enum_multi | [] | no | save | Tagi do filtrowania/debugowania. |
| lifecycle | created_at | timestamp/null | null | no | save | Opcjonalny czas utworzenia. |
| lifecycle | updated_at | timestamp/null | null | no | save | Opcjonalny czas ostatniej zmiany. |
| lifecycle | source | enum | `manual` | yes | save | `manual`, `import`, `preset`, `node_graph`, `generated`. |
| hierarchy | parent_id | id_ref/null | null | no | save | Rodzic w drzewie sceny/UI. |
| hierarchy | layer_id | id_ref | default | yes | save | Warstwa logiczna. |
| hierarchy | z_index | integer | 0 | yes | save | Kolejność w render/hit-test stacku. |
| hierarchy | group_id | id_ref/null | null | no | save | Grupa logiczna. |
| transform | coordinate_space | enum | `parent` | yes | save | `parent`, `canvas`, `viewport`, `screen`, `normalized`. |
| transform | transform_order | enum | `translate_rotate_scale` | yes | save | Kolejność transformacji. |
| transform | x | px | 0 | yes | save | Pozycja X kotwicy lub początku obszaru rezerwacji. |
| transform | y | px | 0 | yes | save | Pozycja Y kotwicy lub początku obszaru rezerwacji. |
| transform | width | px | 0 | yes | save | Szerokość rezerwowanego obszaru; może być 0. |
| transform | height | px | 0 | yes | save | Wysokość rezerwowanego obszaru; może być 0. |
| transform | rotation | angle_deg | 0 | yes | save | Obrót obszaru/kotwicy. |
| transform | scale_x | float | 1 | yes | save | Skala lokalna X. |
| transform | scale_y | float | 1 | yes | save | Skala lokalna Y. |
| transform | pivot_x | percent | 0.5 | yes | save | Punkt obrotu/skalowania X. |
| transform | pivot_y | percent | 0.5 | yes | save | Punkt obrotu/skalowania Y. |
| computed | world_bounds | rect/null | null | no | runtime | Wyliczane bounds po transformacji. Nie zapisywać jako source of truth. |
| computed | screen_bounds | rect/null | null | no | runtime | Bounds po kamerze/zoomie. |
| computed | computed_snap_points | point_array | [] | no | runtime | Punkty snap po transformacji. |
| layout | participates_in_layout | boolean | false | yes | save | Czy empty ma wpływać na layout. Jaśniejsze niż samo `reserve_space`. |
| layout | reserve_space | boolean | false | yes | save | Czy element zajmuje miejsce w layout flow. |
| layout | collapse_when_empty | boolean | true | yes | save | Czy znika z layoutu, gdy nie ma dzieci/roli. |
| layout | min_width | px | 0 | yes | save | Minimalna szerokość, jeśli rezerwuje miejsce. |
| layout | min_height | px | 0 | yes | save | Minimalna wysokość, jeśli rezerwuje miejsce. |
| layout | max_width | px/null | null | no | save | Opcjonalny limit szerokości. |
| layout | max_height | px/null | null | no | save | Opcjonalny limit wysokości. |
| anchor | anchor_mode | enum | `free` | yes | save | `free`, `parent`, `viewport`, `grid`, `object`. |
| anchor | anchor_target_id | id_ref/null | null | no | save | Cel kotwicy, jeśli `anchor_mode` tego wymaga. |
| anchor | anchor_offset_x | px | 0 | yes | save | Offset X względem targetu. |
| anchor | anchor_offset_y | px | 0 | yes | save | Offset Y względem targetu. |
| anchor | anchor_axis | enum | `both` | yes | save | `x`, `y`, `both`, `none`. |
| placeholder | role | enum | `placeholder` | yes | save | `placeholder`, `anchor`, `spacer`, `marker`, `future`, `debug`. |
| placeholder | placeholder_expected_type | id_ref/null | null | no | save | Jaki typ ma później zastąpić empty. |
| placeholder | replacement_policy | enum | `replace` | yes | save | `replace`, `wrap`, `attach_child`, `convert`. |
| behavior | enabled | boolean | true | yes | save | Czy działa jako referencja/logiczny obiekt. |
| behavior | locked | boolean | false | yes | save | Blokuje edycję transformacji i relacji. |
| behavior | selectable | boolean | true | yes | save | Czy można zaznaczyć w outlinerze/canvasie. |
| behavior | hit_test_mode | enum | `none` | yes | save | `none`, `bounds`, `anchor`, `debug_only`. |
| behavior | accepts_children | boolean | false | yes | save | Domyślnie false; gdy true, sprawdzić czy nie powinien być `region`. |
| behavior | receives_events | boolean | false | yes | save | Czy może odbierać eventy runtime/editor. |
| behavior | emits_events | boolean | false | yes | save | Czy może emitować eventy. |
| behavior | cursor_on_hover | enum/null | null | no | save | Kursor edytora przy hover. |
| snapping | snap_enabled | boolean | true | yes | save | Czy może być kandydatem snapowania. |
| snapping | snap_points | enum_multi | [`origin`] | yes | save | `origin`, `center`, `corners`, `edges`. |
| snapping | snap_priority | integer | 0 | yes | save | Priorytet względem innych punktów snap. |
| visual | visible | boolean | false | yes | save | Domyślnie nie renderuje się w normalnym widoku. |
| visual | debug_visible | boolean | true | yes | save | Widoczny w debug/inspect view. |
| visual | debug_shape | enum | `cross` | yes | save | `cross`, `dot`, `bounds`, `label`, `ghost_rect`. |
| visual | debug_label_text | string/null | null | no | save | Opcjonalny tekst debug label. |
| visual | debug_color | color | `#4a9bff` | yes | save | Kolor debug markerów. |
| visual | opacity | percent | 1 | yes | save | Dotyczy tylko debug rendering. |
| state | states | enum_multi | [`default`] | yes | runtime/save | Multi-state: `default`, `selected`, `hover`, `disabled`, `hidden`, `debug`, `dirty`, `warning`, `error`. |
| state | primary_state | enum | `default` | yes | runtime | Dominujący stan wizualny po rozwiązaniu priorytetów. |
| state | state_overrides | json_object | {} | no | runtime/save | Nadpisania zależne od stanu. |
| validation | required_role | boolean | false | yes | save | Czy brak roli jest błędem. |
| validation | allow_zero_size | boolean | true | yes | save | Empty może mieć rozmiar 0×0. |
| validation | participates_in_export | boolean | false | yes | save | Czy empty ma być eksportowany. Domyślnie nie. |
| persistence | save_in_project | boolean | true | yes | save | Czy zapisuje się w pliku projektu. |
| persistence | migration_notes | string/null | null | no | docs | Notatka dla migracji przyszłych wersji. |
| metadata | metadata | json_object | {} | no | save | Dane dodatkowe bez wpływu na renderer. |
| metadata | custom_props | json_object | {} | no | save | Rozszerzenia aplikacji budowanych na engine. |
| editor | editor_flags | json_object | {} | no | editor | `expanded`, `pinned`, `inspector_hidden`, etc. |
| runtime | dirty_flags | enum_multi | [] | no | runtime | `transform`, `layout`, `snap`, `debug`, `metadata`. |

## Implementation contract

| obszar | kontrakt |
|---|---|
| TypeScript | `EmptyPrimitive extends PrimitiveBase` z discriminantem `primitive_type`. |
| Required fields | `schema_version`, `id`, `primitive_type`, `name`, `coordinate_space`, `x`, `y`, `width`, `height`, `states`. |
| Optional fields | `description`, `notes`, `tags`, `metadata`, `custom_props`, anchor target, placeholder expected type. |
| Computed fields | `world_bounds`, `screen_bounds`, `computed_snap_points`, `primary_state`, `dirty_flags`. |
| Renderer | Brak renderu w normal view; debug renderer rysuje `debug_shape`. |
| Hit-test | Domyślnie `none`; `anchor` testuje punkt/mały promień; `bounds` testuje width/height. |
| Snap | `computed_snap_points` powstaje z `snap_points`, transformacji i anchor role. |
| Inspector | Pokazywać role, anchor, layout participation, debug view, replacement policy. |
| Validation | Jeśli `anchor_mode != free`, wymagaj `anchor_target_id` albo valid mode bez targetu. Jeśli `participates_in_layout`, sprawdzaj min/max. |
| Serialization | Zapisywać pola `save`; nie zapisywać runtime computed fields. |
| Migration | `schema_version` decyduje o migracji nazw pól i defaultów. |

## Relacje z innymi systemami

| system | relacja |
|---|---|
| `primitive.region` | Empty przechodzi w region, jeśli zaczyna mieć realny obszar hit/clip/dock/layout. |
| `primitive.rectangle` | Empty przechodzi w rectangle, jeśli ma być widzialnym shape. |
| `layout_engine` | Używa tylko wtedy, gdy `participates_in_layout` albo `reserve_space`. |
| `snap_engine` | Używa `snap_enabled`, `snap_points`, `snap_priority`, `computed_snap_points`. |
| `node_graph` | Może być targetem lub markerem referencyjnym, ale nie powinien wykonywać ciężkiej logiki. |
| `debug_overlay` | Główne miejsce widoczności empty. |

## Zasada

`primitive.empty` ma pozostać lekkim obiektem referencyjnym. Jeśli element ma realny obszar roboczy, maskę, clipping, docking albo hit-zone, użyj `primitive.region`. Jeśli ma widzialny kształt, użyj `primitive.rectangle`, `primitive.curve` albo kolejnego prymitywu wizualnego.
