# Primitive: Rectangle

Status: concept reference → schema-ready reference  
Canonical id: `primitive.rectangle`  
Discriminant field: `primitive_type: "primitive.rectangle"`  
Schema version: `1`

`primitive.rectangle` to podstawowy widzialny kształt prostokątny: panel, karta, tło buttona, ramka, overlay, highlight, placeholder wizualny albo prosta powierzchnia UI. Jest bazą dla większości kompaktowych elementów qubok_interface_core.

## Core intent

| aspekt | decyzja |
|---|---|
| Główna rola | Widzialny prostokąt / rounded rectangle. |
| Render normalny | Fill, stroke, opacity, radius, opcjonalnie shadow. |
| Layout | Może być czystym shape albo visual container. |
| Hit-test | Domyślnie po fill/bounds. |
| Dzieci | Dozwolone tylko gdy `container_mode != none`. Dla czystych obszarów layoutowych preferuj `primitive.region`. |

## Parametry

| grupa | parametr | typ wartości | domyślnie | wymagane | zapis / runtime | opis / reguła |
|---|---|---:|---:|---:|---|---|
| base | schema_version | integer | 1 | yes | save | Wersja schematu do migracji. |
| identity | id | id_ref | auto | yes | save | Unikalny identyfikator instancji. |
| identity | name | string | `Rectangle` | yes | save | Nazwa w outlinerze/inspektorze. |
| identity | primitive_type | enum | `primitive.rectangle` | yes | save | Stały typ prymitywu. |
| identity | description | long_text | empty | no | save | Opis techniczny lub projektowy. |
| identity | notes | long_text | empty | no | save | Notatka użytkownika. |
| identity | tags | enum_multi | [] | no | save | Tagi do filtrowania i wyszukiwania. |
| lifecycle | created_at | timestamp/null | null | no | save | Opcjonalny czas utworzenia. |
| lifecycle | updated_at | timestamp/null | null | no | save | Opcjonalny czas ostatniej zmiany. |
| lifecycle | source | enum | `manual` | yes | save | `manual`, `import`, `preset`, `node_graph`, `generated`. |
| hierarchy | parent_id | id_ref/null | null | no | save | Rodzic w drzewie sceny/UI. |
| hierarchy | layer_id | id_ref | default | yes | save | Warstwa logiczna. |
| hierarchy | z_index | integer | 0 | yes | save | Kolejność rysowania i hit-testu. |
| hierarchy | group_id | id_ref/null | null | no | save | Grupa logiczna. |
| transform | coordinate_space | enum | `parent` | yes | save | `parent`, `canvas`, `viewport`, `screen`, `normalized`. |
| transform | transform_order | enum | `translate_rotate_scale` | yes | save | Kolejność transformacji. |
| transform | x | px | 0 | yes | save | Pozycja lewej krawędzi. |
| transform | y | px | 0 | yes | save | Pozycja górnej krawędzi. |
| transform | width | px | 120 | yes | save | Szerokość. Walidacja: > 0. |
| transform | height | px | 64 | yes | save | Wysokość. Walidacja: > 0. |
| transform | rotation | angle_deg | 0 | yes | save | Obrót względem pivotu. |
| transform | scale_x | float | 1 | yes | save | Skala lokalna X. |
| transform | scale_y | float | 1 | yes | save | Skala lokalna Y. |
| transform | pivot_x | percent | 0.5 | yes | save | Punkt transformacji X. |
| transform | pivot_y | percent | 0.5 | yes | save | Punkt transformacji Y. |
| computed | world_bounds | rect/null | null | no | runtime | Bounds po transformacji. Nie zapisywać jako source of truth. |
| computed | screen_bounds | rect/null | null | no | runtime | Bounds po kamerze/zoomie. |
| computed | inner_bounds | rect/null | null | no | runtime | Bounds po odjęciu stroke. |
| computed | content_bounds | rect/null | null | no | runtime | Bounds po odjęciu stroke i paddingu. |
| computed | resolved_fill_color | color/null | null | no | runtime | Kolor po rozwiązaniu tokena/stanu. |
| computed | resolved_stroke_color | color/null | null | no | runtime | Kolor obrysu po rozwiązaniu tokena/stanu. |
| geometry | corner_radius | px | 4 | yes | save | Główne zaokrąglenie rogów. |
| geometry | corner_radius_tl | px/null | null | no | save | Nadpisanie lewego górnego rogu. |
| geometry | corner_radius_tr | px/null | null | no | save | Nadpisanie prawego górnego rogu. |
| geometry | corner_radius_br | px/null | null | no | save | Nadpisanie prawego dolnego rogu. |
| geometry | corner_radius_bl | px/null | null | no | save | Nadpisanie lewego dolnego rogu. |
| geometry | corner_mode | enum | `circular` | yes | save | `circular`, `continuous`, `beveled`, `sharp`. |
| geometry | corner_smoothing | percent | 0 | yes | save | Dodatkowa miękkość przejścia, jeśli tryb wspiera. |
| geometry | clamp_radius_to_size | boolean | true | yes | save | Radius nie może przekroczyć połowy krótszego boku. |
| fill | fill_enabled | boolean | true | yes | save | Czy rysować wypełnienie. |
| fill | fill_mode | enum | `solid` | yes | save | `none`, `solid`, `token`, `gradient`, `image`. |
| fill | fill_color | color | `#101820` | yes | save | Kolor wypełnienia, jeśli brak tokena. |
| fill | fill_token | id_ref/null | `color.panel.bg` | no | save | Token designu dla wypełnienia. |
| fill | fill_opacity | percent | 1 | yes | save | Przezroczystość wypełnienia. |
| fill | gradient_ref | id_ref/null | null | no | save | Opcjonalny gradient. |
| fill | image_ref | id_ref/null | null | no | save | Opcjonalne image fill. |
| stroke | stroke_enabled | boolean | true | yes | save | Czy rysować obrys. |
| stroke | stroke_color | color | `#2b3a48` | yes | save | Kolor obrysu, jeśli brak tokena. |
| stroke | stroke_token | id_ref/null | `color.border.default` | no | save | Token designu dla obrysu. |
| stroke | stroke_width | px | 1 | yes | save | Grubość obrysu. |
| stroke | stroke_alignment | enum | `inside` | yes | save | `inside`, `center`, `outside`. |
| stroke | stroke_sides | enum_multi | [`top`,`right`,`bottom`,`left`] | yes | save | Które krawędzie mają obrys. |
| stroke | stroke_dash | string/null | null | no | save | Wzór linii przerywanej, np. `4 4`. |
| stroke | stroke_radius_join_policy | enum | `follow_radius` | yes | save | `follow_radius`, `miter_at_corners`, `separate_sides`. |
| effects | opacity | percent | 1 | yes | save | Globalna przezroczystość elementu. |
| effects | blend_mode | enum | `normal` | yes | save | Tryb mieszania, jeśli renderer wspiera. |
| effects | shadow_enabled | boolean | false | yes | save | Domyślnie bez cienia dla minimal style. |
| effects | shadow_token | id_ref/null | null | no | save | Token cienia, jeśli używany. |
| layout | layout_role | enum | `shape` | yes | save | `shape`, `background`, `panel_bg`, `button_bg`, `highlight`, `debug_box`. |
| layout | container_mode | enum | `none` | yes | save | `none`, `visual_container`, `clipping_container`. |
| layout | accepts_children | boolean | false | yes | save | Powinno być true tylko przy container_mode innym niż none. |
| layout | layout_size_mode | enum | `fixed` | yes | save | `fixed`, `hug`, `fill`, `auto`. |
| layout | clip_content | boolean | false | yes | save | Czy dzieci są przycinane do kształtu. |
| layout | padding_left | px | 0 | yes | save | Padding dla dzieci. |
| layout | padding_right | px | 0 | yes | save | Padding dla dzieci. |
| layout | padding_top | px | 0 | yes | save | Padding dla dzieci. |
| layout | padding_bottom | px | 0 | yes | save | Padding dla dzieci. |
| constraints | min_width | px | 1 | yes | save | Minimalna szerokość. |
| constraints | min_height | px | 1 | yes | save | Minimalna wysokość. |
| constraints | max_width | px/null | null | no | save | Opcjonalny limit szerokości. |
| constraints | max_height | px/null | null | no | save | Opcjonalny limit wysokości. |
| constraints | preserve_aspect_ratio | boolean | false | yes | save | Jeśli true, resize trzyma proporcje. |
| behavior | enabled | boolean | true | yes | save | Czy element działa w runtime. |
| behavior | visible | boolean | true | yes | save | Czy jest renderowany. |
| behavior | locked | boolean | false | yes | save | Blokuje edycję. |
| behavior | selectable | boolean | true | yes | save | Czy można zaznaczyć. |
| behavior | resizable | boolean | true | yes | save | Czy można zmieniać rozmiar. |
| behavior | resize_handles | enum_multi | [`n`,`e`,`s`,`w`,`ne`,`se`,`sw`,`nw`] | yes | save | Aktywne uchwyty resize. |
| behavior | hit_test_mode | enum | `fill_bounds` | yes | save | `none`, `bounds`, `fill_bounds`, `stroke`, `debug_only`. |
| behavior | receives_events | boolean | true | yes | save | Czy może odbierać eventy. |
| behavior | emits_events | boolean | false | yes | save | Czy może emitować eventy. |
| behavior | cursor_on_hover | enum/null | null | no | save | Kursor edytora przy hover. |
| snapping | snap_enabled | boolean | true | yes | save | Czy bierze udział w snapowaniu. |
| snapping | snap_points | enum_multi | [`corners`,`center`,`edges`] | yes | save | Punkty snapowania. |
| snapping | snap_priority | integer | 0 | yes | save | Priorytet snapowania. |
| computed | computed_snap_points | point_array | [] | no | runtime | Punkty snap po transformacji. |
| state | states | enum_multi | [`default`] | yes | runtime/save | Multi-state: `default`, `hover`, `selected`, `focused`, `disabled`, `error`, `warning`, `dirty`, `hidden`. |
| state | primary_state | enum | `default` | yes | runtime | Dominujący stan wizualny. |
| state | state_overrides | json_object | {} | no | runtime/save | Nadpisania fill/stroke/opacity/cursor zależne od stanu. |
| debug | debug_show_bounds | boolean | false | yes | save | Pokazuje bounds niezależnie od stylu. |
| debug | debug_show_pivot | boolean | false | yes | save | Pokazuje pivot. |
| debug | debug_label | boolean | false | yes | save | Pokazuje nazwę/id. |
| export | export_role | enum | `rect` | yes | save | `rect`, `frame`, `background`, `mask`, `ignore`. |
| validation | require_positive_size | boolean | true | yes | save | Rectangle wymaga width/height > 0. |
| validation | auto_clamp_radius | boolean | true | yes | save | Automatycznie ogranicza radius. |
| persistence | save_in_project | boolean | true | yes | save | Czy zapisuje się w projekcie. |
| persistence | migration_notes | string/null | null | no | docs | Notatka migracyjna. |
| metadata | metadata | json_object | {} | no | save | Dane dodatkowe bez wpływu na renderer. |
| metadata | custom_props | json_object | {} | no | save | Rozszerzenia aplikacji na engine. |
| editor | editor_flags | json_object | {} | no | editor | `expanded`, `pinned`, `inspector_hidden`, etc. |
| runtime | dirty_flags | enum_multi | [] | no | runtime | `transform`, `geometry`, `style`, `layout`, `snap`, `render`. |

## Implementation contract

| obszar | kontrakt |
|---|---|
| TypeScript | `RectanglePrimitive extends PrimitiveBase`. |
| Required fields | `schema_version`, `id`, `primitive_type`, `name`, transform, `width`, `height`, geometry, fill/stroke flags, states. |
| Optional fields | Per-corner radius, token refs, gradient/image refs, shadow, custom props. |
| Computed fields | `world_bounds`, `screen_bounds`, `inner_bounds`, `content_bounds`, resolved colors, computed snap points. |
| Renderer | Rysuje rounded rectangle z fill/stroke/effects; rozwiązuje tokeny przed renderem. |
| Hit-test | `fill_bounds` testuje kształt po radius; `bounds` testuje prosty AABB; `stroke` testuje obrys. |
| Snap | `corners`, `center`, `edges` po transformacji i skali. |
| Inspector | Sekcje: Transform, Geometry, Fill, Stroke, Layout, Behavior, Debug, Export. |
| Validation | `width > 0`, `height > 0`, `radius <= min(width,height)/2`, `accepts_children` zgodne z `container_mode`. |
| Serialization | Zapisywać pola `save`; runtime computed fields odtwarzać po load. |
| Migration | `schema_version` steruje zmianami nazw pól, defaultów i token references. |

## Relacje z innymi systemami

| system | relacja |
|---|---|
| `design_tokens` | `fill_token`, `stroke_token`, `shadow_token`. |
| `layout_engine` | Używa `layout_size_mode`, paddingów i `container_mode`. |
| `hit_test` | Używa `hit_test_mode`, transformu i geometry radius. |
| `snap_engine` | Używa snap points po transformacji. |
| `inspector` | Generuje edytory z typów wartości i grup parametrów. |
| `node_graph` | Może być targetem akcji: set fill, set stroke, resize, move, show/hide. |

## Zasada

`primitive.rectangle` jest widzialnym kształtem. Jeśli element jest tylko niewidzialnym obszarem odpowiedzialności, docking targetem, clipping/hit-zone albo safe area, użyj `primitive.region`. Jeśli rectangle ma dzieci, musi jawnie ustawić `container_mode`.
