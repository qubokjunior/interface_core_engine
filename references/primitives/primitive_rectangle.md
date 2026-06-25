# Primitive: Rectangle

`primitive.rectangle` to podstawowy widzialny kształt prostokątny: panel, karta, tło buttona, ramka, overlay, highlight, placeholder wizualny albo prosta powierzchnia UI. Jest bazą dla większości kompaktowych elementów qubok_interface_core.

## Parametry

| grupa | parametr | typ wartości | domyślnie | opis / reguła |
|---|---|---:|---:|---|
| identity | id | id_ref | auto | Unikalny identyfikator instancji. |
| identity | name | string | `Rectangle` | Nazwa widoczna w outlinerze/inspektorze. |
| identity | primitive_type | enum | `primitive.rectangle` | Stały typ prymitywu. |
| identity | description | long_text | empty | Opcjonalna notatka projektowa. |
| identity | tags | enum_multi | [] | Tagi do filtrowania i wyszukiwania. |
| hierarchy | parent_id | id_ref/null | null | Rodzic w drzewie sceny/UI. |
| hierarchy | layer_id | id_ref | default | Warstwa logiczna. |
| hierarchy | z_index | integer | 0 | Kolejność rysowania i hit-testu. |
| hierarchy | group_id | id_ref/null | null | Grupa logiczna. |
| transform | x | px | 0 | Pozycja lewej krawędzi. |
| transform | y | px | 0 | Pozycja górnej krawędzi. |
| transform | width | px | 120 | Szerokość. Walidacja: > 0. |
| transform | height | px | 64 | Wysokość. Walidacja: > 0. |
| transform | rotation | float_deg | 0 | Obrót względem pivotu. |
| transform | scale_x | float | 1 | Skala lokalna X. |
| transform | scale_y | float | 1 | Skala lokalna Y. |
| transform | pivot_x | percent | 0.5 | Punkt transformacji X. |
| transform | pivot_y | percent | 0.5 | Punkt transformacji Y. |
| geometry | corner_radius | px | 4 | Główne zaokrąglenie rogów. |
| geometry | corner_radius_tl | px/null | null | Nadpisanie lewego górnego rogu. |
| geometry | corner_radius_tr | px/null | null | Nadpisanie prawego górnego rogu. |
| geometry | corner_radius_br | px/null | null | Nadpisanie prawego dolnego rogu. |
| geometry | corner_radius_bl | px/null | null | Nadpisanie lewego dolnego rogu. |
| geometry | corner_mode | enum | `circular` | `circular`, `continuous`, `beveled`, `sharp`. |
| geometry | corner_smoothing | percent | 0 | Dodatkowa miękkość przejścia, jeśli tryb wspiera. |
| geometry | clamp_radius_to_size | boolean | true | Radius nie może przekroczyć połowy krótszego boku. |
| fill | fill_enabled | boolean | true | Czy rysować wypełnienie. |
| fill | fill_color | color | `#101820` | Kolor wypełnienia, jeśli brak tokena. |
| fill | fill_token | id_ref/null | `color.panel.bg` | Token designu dla wypełnienia. |
| fill | fill_opacity | percent | 1 | Przezroczystość wypełnienia. |
| stroke | stroke_enabled | boolean | true | Czy rysować obrys. |
| stroke | stroke_color | color | `#2b3a48` | Kolor obrysu, jeśli brak tokena. |
| stroke | stroke_token | id_ref/null | `color.border.default` | Token designu dla obrysu. |
| stroke | stroke_width | px | 1 | Grubość obrysu. |
| stroke | stroke_alignment | enum | `inside` | `inside`, `center`, `outside`. |
| stroke | stroke_dash | string/null | null | Wzór linii przerywanej, np. `4 4`. |
| effects | opacity | percent | 1 | Globalna przezroczystość elementu. |
| effects | blend_mode | enum | `normal` | Tryb mieszania, jeśli renderer wspiera. |
| effects | shadow_enabled | boolean | false | Domyślnie bez cienia dla minimal style. |
| effects | shadow_token | id_ref/null | null | Token cienia, jeśli używany. |
| layout | layout_role | enum | `shape` | `shape`, `background`, `panel_bg`, `button_bg`, `highlight`, `debug_box`. |
| layout | clip_content | boolean | false | Czy dzieci są przycinane do kształtu. |
| layout | padding_left | px | 0 | Padding dla dzieci, jeśli rectangle działa jako kontener. |
| layout | padding_right | px | 0 | Padding dla dzieci. |
| layout | padding_top | px | 0 | Padding dla dzieci. |
| layout | padding_bottom | px | 0 | Padding dla dzieci. |
| constraints | min_width | px | 1 | Minimalna szerokość. |
| constraints | min_height | px | 1 | Minimalna wysokość. |
| constraints | max_width | px/null | null | Opcjonalny limit szerokości. |
| constraints | max_height | px/null | null | Opcjonalny limit wysokości. |
| constraints | preserve_aspect_ratio | boolean | false | Jeśli true, resize trzyma proporcje. |
| behavior | enabled | boolean | true | Czy element działa w runtime. |
| behavior | visible | boolean | true | Czy jest renderowany. |
| behavior | locked | boolean | false | Blokuje edycję. |
| behavior | selectable | boolean | true | Czy można zaznaczyć. |
| behavior | resizable | boolean | true | Czy można zmieniać rozmiar. |
| behavior | hit_test_mode | enum | `fill_bounds` | `none`, `bounds`, `fill_bounds`, `stroke`, `debug_only`. |
| snapping | snap_enabled | boolean | true | Czy bierze udział w snapowaniu. |
| snapping | snap_points | enum_multi | [`corners`,`center`,`edges`] | Punkty snapowania. |
| state | state | enum | `default` | `default`, `hover`, `selected`, `disabled`, `error`, `hidden`. |
| debug | debug_show_bounds | boolean | false | Pokazuje bounds niezależnie od stylu. |
| debug | debug_show_pivot | boolean | false | Pokazuje pivot. |
| debug | debug_label | boolean | false | Pokazuje nazwę/id. |
| persistence | save_in_project | boolean | true | Czy zapisuje się w projekcie. |
| metadata | metadata | json_object | {} | Dane dodatkowe bez wpływu na renderer. |

## Zasada

`primitive.rectangle` jest widzialnym kształtem. Nie powinien przejmować roli regionu layoutowego, jeśli nie potrzebuje wypełnienia/obrysu. Dla obszarów niewidzialnych, dockingowych, safe-area, clipping/hit-zone użyj `primitive.region`.
