# Primitive: Region

Status: concept reference → schema-ready reference  
Canonical id: `primitive.region`  
Discriminant field: `primitive_type: "primitive.region"`  
Schema version: `1`

`primitive.region` to logiczny obszar roboczy/interakcyjny: safe area, content region, dock target, hit-zone, clipping region, drop-zone, layout boundary, scroll area albo debug overlay. Region może być niewidoczny w normalnym widoku, ale ma realny wpływ na layout, hit-test, constraints, docking i przepływ dzieci.

## Core intent

| aspekt | decyzja |
|---|---|
| Główna rola | Obszar odpowiedzialności, nie dekoracja. |
| Render normalny | Domyślnie niewidoczny. |
| Render debug | Outline/fill/label regionu. |
| Layout | Może zarządzać dziećmi przez capability `layout`. |
| Clipping / scroll | Aktywne tylko przez capability. |
| Docking / drop | Aktywne tylko przez capability. |
| Hit-test | Domyślnie po bounds, ale może działać jako children-only/debug-only. |

## Capability model

Region jest szeroki, dlatego implementacyjnie musi działać przez jawne capability flags. Bez tego jeden typ zacznie mieszać layout, clipping, docking, scroll, drop-zone i debug overlay.

| capability | domyślnie | aktywuje |
|---|---:|---|
| `layout` | true | `layout_mode`, flow, align, gap, padding, wrap. |
| `clipping` | false | `clip_enabled`, overflow, clip shape. |
| `scrolling` | false | scroll positions, content size, scrollbar visibility. |
| `docking` | false | dock position, dock accepts, snap rule, preview mode. |
| `drop_target` | false | drop accepts, payload validation. |
| `hit_zone` | true | hit-test regionu. |
| `debug_overlay` | true | debug fill/stroke/label. |

## Parametry

| grupa | parametr | typ wartości | domyślnie | wymagane | zapis / runtime | opis / reguła |
|---|---|---:|---:|---:|---|---|
| base | schema_version | integer | 1 | yes | save | Wersja schematu do migracji. |
| identity | id | id_ref | auto | yes | save | Unikalny identyfikator instancji. |
| identity | name | string | `Region` | yes | save | Nazwa w outlinerze/inspektorze. |
| identity | primitive_type | enum | `primitive.region` | yes | save | Stały typ prymitywu. |
| identity | description | long_text | empty | no | save | Opis techniczny lub projektowy. |
| identity | notes | long_text | empty | no | save | Notatka użytkownika. |
| identity | tags | enum_multi | [] | no | save | Tagi do filtrowania i debugowania. |
| lifecycle | created_at | timestamp/null | null | no | save | Opcjonalny czas utworzenia. |
| lifecycle | updated_at | timestamp/null | null | no | save | Opcjonalny czas ostatniej zmiany. |
| lifecycle | source | enum | `manual` | yes | save | `manual`, `import`, `preset`, `node_graph`, `generated`. |
| hierarchy | parent_id | id_ref/null | null | no | save | Rodzic w drzewie sceny/UI. |
| hierarchy | layer_id | id_ref | default | yes | save | Warstwa logiczna. |
| hierarchy | z_index | integer | 0 | yes | save | Kolejność regionów w hit-test i overlay. |
| hierarchy | group_id | id_ref/null | null | no | save | Grupa logiczna. |
| transform | coordinate_space | enum | `parent` | yes | save | `parent`, `canvas`, `viewport`, `screen`, `normalized`. |
| transform | transform_order | enum | `translate_rotate_scale` | yes | save | Kolejność transformacji. |
| transform | x | px | 0 | yes | save | Pozycja lewej krawędzi regionu. |
| transform | y | px | 0 | yes | save | Pozycja górnej krawędzi regionu. |
| transform | width | px | 320 | yes | save | Szerokość regionu. Walidacja: > 0. |
| transform | height | px | 240 | yes | save | Wysokość regionu. Walidacja: > 0. |
| transform | rotation | angle_deg | 0 | yes | save | Obrót regionu; layout może go ignorować. |
| transform | scale_x | float | 1 | yes | save | Skala X. |
| transform | scale_y | float | 1 | yes | save | Skala Y. |
| transform | pivot_x | percent | 0.5 | yes | save | Pivot X. |
| transform | pivot_y | percent | 0.5 | yes | save | Pivot Y. |
| computed | world_bounds | rect/null | null | no | runtime | Bounds po transformacji. |
| computed | screen_bounds | rect/null | null | no | runtime | Bounds po kamerze/zoomie. |
| computed | content_bounds | rect/null | null | no | runtime | Bounds po paddingu. |
| computed | computed_snap_points | point_array | [] | no | runtime | Punkty snap po transformacji. |
| computed | layout_invalid_reason | string/null | null | no | runtime | Powód błędu layoutu. |
| capabilities | capabilities | json_object | `{layout:true, hit_zone:true, debug_overlay:true}` | yes | save | Jawne moduły regionu. |
| region | region_role | enum | `content` | yes | save | `content`, `safe_area`, `dock`, `drop_zone`, `hit_zone`, `clip`, `scroll`, `debug`, `bounds`. |
| region | region_coordinate_space | enum | `parent` | yes | save | Przestrzeń interpretacji regionu. |
| region | accepts_children | boolean | true | yes | save | Czy region może zawierać elementy. |
| region | allowed_child_types | enum_multi | [`any`] | yes | save | Lista dozwolonych typów dzieci. |
| region | rejected_child_types | enum_multi | [] | yes | save | Typy zabronione. |
| region | children_order_mode | enum | `z_index_then_tree` | yes | save | `tree`, `z_index`, `z_index_then_tree`, `manual`. |
| region | active_child_id | id_ref/null | null | no | save | Aktywne dziecko, jeśli region przełącza widoki. |
| layout | layout_mode | enum | `free` | yes | save | `free`, `row`, `column`, `grid`, `stack`, `dock`, `absolute`. |
| layout | layout_algorithm | enum | `manual` | yes | save | `manual`, `auto`, `computed`, `external`. |
| layout | flow_direction | enum | `none` | yes | save | `none`, `horizontal`, `vertical`. |
| layout | align_items | enum | `start` | yes | save | `start`, `center`, `end`, `stretch`. |
| layout | justify_content | enum | `start` | yes | save | `start`, `center`, `end`, `space_between`. |
| layout | gap | px | 8 | yes | save | Odstęp między dziećmi. |
| layout | padding_left | px | 8 | yes | save | Wewnętrzny margines lewy. |
| layout | padding_right | px | 8 | yes | save | Wewnętrzny margines prawy. |
| layout | padding_top | px | 8 | yes | save | Wewnętrzny margines górny. |
| layout | padding_bottom | px | 8 | yes | save | Wewnętrzny margines dolny. |
| layout | margin_left | px | 0 | yes | save | Zewnętrzny margines lewy. |
| layout | margin_right | px | 0 | yes | save | Zewnętrzny margines prawy. |
| layout | margin_top | px | 0 | yes | save | Zewnętrzny margines górny. |
| layout | margin_bottom | px | 0 | yes | save | Zewnętrzny margines dolny. |
| layout | wrap | boolean | false | yes | save | Czy dzieci zawijają się w flow/grid. |
| clipping | clip_enabled | boolean | false | yes | save | Czy dzieci są przycinane do regionu. Wymaga capability `clipping`. |
| clipping | clip_shape | enum | `rectangle` | yes | save | `rectangle`, `rounded_rect`, `custom_path`. |
| clipping | clip_path_ref | id_ref/null | null | no | save | Ścieżka clip, jeśli `custom_path`. |
| clipping | overflow_x | enum | `visible` | yes | save | `visible`, `hidden`, `scroll`, `auto`. |
| clipping | overflow_y | enum | `visible` | yes | save | `visible`, `hidden`, `scroll`, `auto`. |
| scrolling | content_width | px/null | null | no | runtime/save | Rozmiar zawartości, jeśli scrolling. |
| scrolling | content_height | px/null | null | no | runtime/save | Rozmiar zawartości, jeśli scrolling. |
| scrolling | scroll_x | px | 0 | yes | save | Pozycja scroll pozioma. |
| scrolling | scroll_y | px | 0 | yes | save | Pozycja scroll pionowa. |
| scrolling | scrollbar_x_visible | boolean | false | yes | runtime/save | Widoczność scrollbara X. |
| scrolling | scrollbar_y_visible | boolean | false | yes | runtime/save | Widoczność scrollbara Y. |
| docking | dock_enabled | boolean | false | yes | save | Czy region przyjmuje dockowane panele. Wymaga capability `docking`. |
| docking | dock_position | enum/null | null | no | save | `left`, `right`, `top`, `bottom`, `center`, `floating`. |
| docking | dock_accepts_panel_types | enum_multi | [`any`] | yes | save | Jakie panele można dokować. |
| docking | dock_priority | integer | 0 | yes | save | Priorytet przy nakładających się dock zones. |
| docking | snap_rule | enum | `inside` | yes | save | `inside`, `edge`, `split`, `replace`, `tab_stack`. |
| docking | preview_mode | enum | `outline` | yes | save | `outline`, `fill`, `split_preview`, `tab_preview`. |
| drop | drop_accepts | enum_multi | [] | yes | save | Typy payloadów drag/drop. Wymaga capability `drop_target`. |
| drop | drop_effect | enum | `move` | yes | save | `copy`, `move`, `link`, `reject`. |
| constraints | min_width | px | 1 | yes | save | Minimalna szerokość. |
| constraints | min_height | px | 1 | yes | save | Minimalna wysokość. |
| constraints | max_width | px/null | null | no | save | Maksymalna szerokość. |
| constraints | max_height | px/null | null | no | save | Maksymalna wysokość. |
| constraints | resize_axis | enum | `both` | yes | save | `none`, `x`, `y`, `both`. |
| behavior | enabled | boolean | true | yes | save | Czy region działa w runtime. |
| behavior | visible | boolean | false | yes | save | Czy region jest widoczny w normalnym widoku. |
| behavior | debug_visible | boolean | true | yes | save | Czy pokazuje outline w debug view. |
| behavior | locked | boolean | false | yes | save | Blokuje edycję. |
| behavior | selectable | boolean | true | yes | save | Czy region można zaznaczyć. |
| behavior | hit_test_mode | enum | `bounds` | yes | save | `none`, `bounds`, `children_only`, `debug_only`. |
| behavior | receives_events | boolean | true | yes | save | Czy może odbierać eventy. |
| behavior | emits_events | boolean | false | yes | save | Czy może emitować eventy. |
| behavior | cursor_on_hover | enum/null | null | no | save | Kursor edytora przy hover. |
| visual | debug_fill | color | `#4a9bff22` | yes | save | Kolor półprzezroczysty w debug overlay. |
| visual | debug_stroke | color | `#4a9bff` | yes | save | Kolor obrysu debug. |
| visual | debug_stroke_width | px | 1 | yes | save | Grubość obrysu debug. |
| visual | debug_label | boolean | true | yes | save | Pokazuje nazwę/rolę regionu. |
| snapping | snap_enabled | boolean | true | yes | save | Czy region jest kandydatem snapowania. |
| snapping | snap_points | enum_multi | [`edges`,`center`] | yes | save | Punkty snapowania. |
| snapping | snap_priority | integer | 0 | yes | save | Priorytet snapowania. |
| state | states | enum_multi | [`default`] | yes | runtime/save | Multi-state: `default`, `hover`, `active`, `selected`, `invalid`, `disabled`, `hidden`, `dirty`, `warning`, `error`. |
| state | primary_state | enum | `default` | yes | runtime | Dominujący stan wizualny. |
| state | state_overrides | json_object | {} | no | runtime/save | Nadpisania debug/stroke/visibility/cursor zależne od stanu. |
| validation | require_positive_size | boolean | true | yes | save | Region musi mieć dodatni rozmiar. |
| validation | validate_capability_consistency | boolean | true | yes | save | Sprawdza zgodność capabilities z polami. |
| persistence | save_in_project | boolean | true | yes | save | Czy zapisuje się w projekcie. |
| persistence | migration_notes | string/null | null | no | docs | Notatka migracyjna. |
| metadata | metadata | json_object | {} | no | save | Dane dodatkowe bez wpływu na renderer. |
| metadata | custom_props | json_object | {} | no | save | Rozszerzenia aplikacji na engine. |
| editor | editor_flags | json_object | {} | no | editor | `expanded`, `pinned`, `inspector_hidden`, etc. |
| runtime | dirty_flags | enum_multi | [] | no | runtime | `transform`, `layout`, `clip`, `scroll`, `dock`, `hit_test`, `debug`. |

## Implementation contract

| obszar | kontrakt |
|---|---|
| TypeScript | `RegionPrimitive extends PrimitiveBase`. |
| Required fields | `schema_version`, `id`, `primitive_type`, `name`, transform, size, capabilities, region role, states. |
| Optional fields | docking, scrolling, clipping, drop, custom props. |
| Computed fields | `world_bounds`, `screen_bounds`, `content_bounds`, `computed_snap_points`, `layout_invalid_reason`. |
| Renderer | Normal view domyślnie nic nie rysuje; debug renderer rysuje fill/stroke/label. |
| Hit-test | `bounds` testuje region; `children_only` ignoruje region i testuje dzieci; `debug_only` działa tylko w debug view. |
| Layout | Aktywny tylko przy `capabilities.layout = true`. |
| Clipping | Aktywne tylko przy `capabilities.clipping = true` i `clip_enabled = true`. |
| Docking | Aktywne tylko przy `capabilities.docking = true` i `dock_enabled = true`. |
| Scroll | Aktywny tylko przy `capabilities.scrolling = true`. |
| Validation | Sprawdza dodatni rozmiar, zgodność capability flags, dock accepts, overflow/scroll i layout mode. |
| Serialization | Zapisywać pola `save`; computed runtime odbudowywać po load. |

## Relacje z innymi systemami

| system | relacja |
|---|---|
| `layout_engine` | Główne miejsce interpretacji regionu jako kontenera. |
| `docking_engine` | Używa `dock_enabled`, `dock_accepts_panel_types`, `snap_rule`, `preview_mode`. |
| `hit_test` | Używa `hit_test_mode`, bounds i capability `hit_zone`. |
| `scroll_runtime` | Używa content size, overflow i scroll positions. |
| `debug_overlay` | Pokazuje niewidzialne regiony i powód błędów layoutu. |
| `node_graph` | Może targetować region: set active child, set visibility, update layout, enable docking. |

## Zasada

`primitive.region` jest obszarem odpowiedzialności, nie dekoracją. Jeśli potrzebujesz widzialnej ramki/tła, użyj `primitive.rectangle`. Jeśli region zaczyna robić zbyt dużo, nie twórz nowego primitive — wyłącz/łącz capabilities.
