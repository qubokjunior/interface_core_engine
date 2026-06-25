# Primitive: Empty

`primitive.empty` to niewidoczny lub debugowo widoczny element bazowy. Służy jako placeholder, punkt/obszar rezerwujący, kotwica layoutu, marker snapowania, przyszły element do wypełnienia albo neutralny obiekt w strukturze projektu.

## Parametry

| grupa | parametr | typ wartości | domyślnie | opis / reguła |
|---|---|---:|---:|---|
| identity | id | id_ref | auto | Unikalny identyfikator instancji. |
| identity | name | string | `Empty` | Nazwa widoczna w outlinerze/inspektorze. |
| identity | primitive_type | enum | `primitive.empty` | Stały typ prymitywu. |
| identity | description | long_text | empty | Opcjonalna notatka projektowa. |
| identity | tags | enum_multi | [] | Tagi do filtrowania, debugowania i wyszukiwania. |
| hierarchy | parent_id | id_ref/null | null | Rodzic w drzewie sceny/UI. |
| hierarchy | layer_id | id_ref | default | Warstwa logiczna. |
| hierarchy | z_index | integer | 0 | Kolejność w render/hit-test stacku. |
| hierarchy | group_id | id_ref/null | null | Grupa logiczna, jeśli element jest częścią zbioru. |
| transform | x | px | 0 | Pozycja lewej krawędzi lub kotwicy. |
| transform | y | px | 0 | Pozycja górnej krawędzi lub kotwicy. |
| transform | width | px | 0 | Szerokość rezerwowanego obszaru. Może być 0. |
| transform | height | px | 0 | Wysokość rezerwowanego obszaru. Może być 0. |
| transform | rotation | float_deg | 0 | Obrót obszaru/kotwicy. |
| transform | scale_x | float | 1 | Skala lokalna X. |
| transform | scale_y | float | 1 | Skala lokalna Y. |
| transform | pivot_x | percent | 0.5 | Punkt obrotu/skalowania X. |
| transform | pivot_y | percent | 0.5 | Punkt obrotu/skalowania Y. |
| layout | reserve_space | boolean | false | Czy element zajmuje miejsce w layout flow. |
| layout | collapse_when_empty | boolean | true | Czy znika z layoutu, gdy nie ma dzieci/roli. |
| layout | min_width | px | 0 | Minimalna szerokość, jeśli rezerwuje miejsce. |
| layout | min_height | px | 0 | Minimalna wysokość, jeśli rezerwuje miejsce. |
| layout | max_width | px/null | null | Opcjonalny limit szerokości. |
| layout | max_height | px/null | null | Opcjonalny limit wysokości. |
| layout | anchor_mode | enum | `free` | `free`, `parent`, `viewport`, `grid`, `object`. |
| layout | anchor_target_id | id_ref/null | null | Cel kotwicy, jeśli anchor_mode tego wymaga. |
| behavior | enabled | boolean | true | Czy element działa jako referencja/logiczny obiekt. |
| behavior | locked | boolean | false | Blokuje edycję transformacji i relacji. |
| behavior | selectable | boolean | true | Czy można zaznaczyć w outlinerze/canvasie. |
| behavior | hit_test_mode | enum | `none` | `none`, `bounds`, `anchor`, `debug_only`. |
| behavior | accepts_children | boolean | false | Czy może tymczasowo zawierać dzieci. |
| behavior | role | enum | `placeholder` | `placeholder`, `anchor`, `spacer`, `marker`, `future`, `debug`. |
| snapping | snap_enabled | boolean | true | Czy może być kandydatem snapowania. |
| snapping | snap_points | enum_multi | [`origin`] | `origin`, `center`, `corners`, `edges`. |
| snapping | snap_priority | integer | 0 | Priorytet względem innych punktów snap. |
| visual | visible | boolean | false | Domyślnie nie renderuje się w normalnym widoku. |
| visual | debug_visible | boolean | true | Widoczny w debug/inspect view. |
| visual | debug_shape | enum | `cross` | `cross`, `dot`, `bounds`, `label`, `ghost_rect`. |
| visual | debug_color | color | `#4a9bff` | Kolor debug markerów. |
| visual | opacity | percent | 1 | Dotyczy tylko debug rendering. |
| state | state | enum | `default` | `default`, `selected`, `hover`, `disabled`, `hidden`, `debug`. |
| validation | required_role | boolean | false | Czy brak roli jest błędem. |
| validation | allow_zero_size | boolean | true | Empty może mieć rozmiar 0×0. |
| persistence | save_in_project | boolean | true | Czy zapisuje się w pliku projektu. |
| metadata | metadata | json_object | {} | Dane dodatkowe bez wpływu na renderer. |

## Zasada

`primitive.empty` nie powinien udawać rectangle/region. Jeśli element ma realny obszar roboczy, maskę, clipping, docking albo hit-zone, użyj `primitive.region`. Jeśli ma widzialny kształt, użyj `primitive.rectangle`, `primitive.curve` albo kolejnego prymitywu wizualnego.
