# Primitive: Region

`primitive.region` to logiczny obszar roboczy/interakcyjny: safe area, content region, dock target, hit-zone, clipping region, drop-zone, layout boundary, scroll area albo debug overlay. Region może być niewidoczny w normalnym widoku, ale ma realny wpływ na layout, hit-test, constraints, docking i przepływ dzieci.

## Parametry

| grupa | parametr | typ wartości | domyślnie | opis / reguła |
|---|---|---:|---:|---|
| identity | id | id_ref | auto | Unikalny identyfikator instancji. |
| identity | name | string | `Region` | Nazwa widoczna w outlinerze/inspektorze. |
| identity | primitive_type | enum | `primitive.region` | Stały typ prymitywu. |
| identity | description | long_text | empty | Opcjonalna notatka projektowa. |
| identity | tags | enum_multi | [] | Tagi do filtrowania i debugowania. |
| hierarchy | parent_id | id_ref/null | null | Rodzic w drzewie sceny/UI. |
| hierarchy | layer_id | id_ref | default | Warstwa logiczna. |
| hierarchy | z_index | integer | 0 | Kolejność regionów w hit-test i overlay. |
| hierarchy | group_id | id_ref/null | null | Grupa logiczna. |
| transform | x | px | 0 | Pozycja lewej krawędzi regionu. |
| transform | y | px | 0 | Pozycja górnej krawędzi regionu. |
| transform | width | px | 320 | Szerokość regionu. Walidacja: > 0. |
| transform | height | px | 240 | Wysokość regionu. Walidacja: > 0. |
| transform | rotation | float_deg | 0 | Obrót regionu, domyślnie niewykorzystywany w layout. |
| transform | scale_x | float | 1 | Skala X. |
| transform | scale_y | float | 1 | Skala Y. |
| transform | pivot_x | percent | 0.5 | Pivot X. |
| transform | pivot_y | percent | 0.5 | Pivot Y. |
| region | region_role | enum | `content` | `content`, `safe_area`, `dock`, `drop_zone`, `hit_zone`, `clip`, `scroll`, `debug`, `bounds`. |
| region | accepts_children | boolean | true | Czy region może zawierać elementy. |
| region | allowed_child_types | enum_multi | [`any`] | Lista dozwolonych typów dzieci. |
| region | rejected_child_types | enum_multi | [] | Typy zabronione. |
| region | active_child_id | id_ref/null | null | Aktywne dziecko, jeśli region przełącza widoki. |
| layout | layout_mode | enum | `free` | `free`, `row`, `column`, `grid`, `stack`, `dock`, `absolute`. |
| layout | flow_direction | enum | `none` | `none`, `horizontal`, `vertical`. |
| layout | align_items | enum | `start` | `start`, `center`, `end`, `stretch`. |
| layout | justify_content | enum | `start` | `start`, `center`, `end`, `space_between`. |
| layout | gap | px | 8 | Odstęp między dziećmi. |
| layout | padding_left | px | 8 | Wewnętrzny margines lewy. |
| layout | padding_right | px | 8 | Wewnętrzny margines prawy. |
| layout | padding_top | px | 8 | Wewnętrzny margines górny. |
| layout | padding_bottom | px | 8 | Wewnętrzny margines dolny. |
| layout | margin_left | px | 0 | Zewnętrzny margines lewy. |
| layout | margin_right | px | 0 | Zewnętrzny margines prawy. |
| layout | margin_top | px | 0 | Zewnętrzny margines górny. |
| layout | margin_bottom | px | 0 | Zewnętrzny margines dolny. |
| layout | wrap | boolean | false | Czy dzieci zawijają się w flow/grid. |
| clipping | clip_enabled | boolean | false | Czy dzieci są przycinane do regionu. |
| clipping | overflow_x | enum | `visible` | `visible`, `hidden`, `scroll`, `auto`. |
| clipping | overflow_y | enum | `visible` | `visible`, `hidden`, `scroll`, `auto`. |
| clipping | scroll_x | px | 0 | Pozycja scroll pozioma. |
| clipping | scroll_y | px | 0 | Pozycja scroll pionowa. |
| docking | dock_enabled | boolean | false | Czy region przyjmuje dockowane panele. |
| docking | dock_position | enum/null | null | `left`, `right`, `top`, `bottom`, `center`, `floating`. |
| docking | dock_priority | integer | 0 | Priorytet przy nakładających się dock zones. |
| docking | snap_rule | enum | `inside` | `inside`, `edge`, `split`, `replace`, `tab_stack`. |
| docking | preview_mode | enum | `outline` | `outline`, `fill`, `split_preview`, `tab_preview`. |
| constraints | min_width | px | 1 | Minimalna szerokość. |
| constraints | min_height | px | 1 | Minimalna wysokość. |
| constraints | max_width | px/null | null | Maksymalna szerokość. |
| constraints | max_height | px/null | null | Maksymalna wysokość. |
| constraints | resize_axis | enum | `both` | `none`, `x`, `y`, `both`. |
| behavior | enabled | boolean | true | Czy region działa w runtime. |
| behavior | visible | boolean | false | Czy region jest widoczny w normalnym widoku. |
| behavior | debug_visible | boolean | true | Czy region pokazuje outline w debug view. |
| behavior | locked | boolean | false | Blokuje edycję. |
| behavior | selectable | boolean | true | Czy region można zaznaczyć. |
| behavior | hit_test_mode | enum | `bounds` | `none`, `bounds`, `children_only`, `debug_only`. |
| visual | debug_fill | color | `#4a9bff22` | Kolor półprzezroczysty w debug overlay. |
| visual | debug_stroke | color | `#4a9bff` | Kolor obrysu debug. |
| visual | debug_stroke_width | px | 1 | Grubość obrysu debug. |
| visual | debug_label | boolean | true | Pokazuje nazwę/rolę regionu. |
| snapping | snap_enabled | boolean | true | Czy region jest kandydatem snapowania. |
| snapping | snap_points | enum_multi | [`edges`,`center`] | Punkty snapowania. |
| state | state | enum | `default` | `default`, `hover`, `active`, `selected`, `invalid`, `disabled`, `hidden`. |
| validation | require_positive_size | boolean | true | Region musi mieć dodatni rozmiar. |
| persistence | save_in_project | boolean | true | Czy zapisuje się w projekcie. |
| metadata | metadata | json_object | {} | Dane dodatkowe bez wpływu na renderer. |

## Zasada

`primitive.region` jest obszarem odpowiedzialności, nie dekoracją. Może być niewidzialny, ale ma znaczenie dla layoutu, snapowania, docking preview, hit-testu i debugowania. Jeśli potrzebujesz tylko widzialnej ramki/tła, użyj `primitive.rectangle`.
