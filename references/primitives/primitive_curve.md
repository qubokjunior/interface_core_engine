# Primitive: Curve

Status: concept reference → schema-ready reference  
Canonical id: `primitive.curve`  
Discriminant field: `primitive_type: "primitive.curve"`  
Schema version: `1`

`primitive.curve` to podstawowy prymityw liniowy/ścieżkowy: linia, krzywa Béziera, polyline, ścieżka edytora curve, connector, guide, granica debug, motion path albo lekki shape oparty na punktach. Curve opisuje geometrię przez punkty, segmenty i styl stroke.

## Core intent

| aspekt | decyzja |
|---|---|
| Główna rola | Ścieżka/polilinia/krzywa/connector/guide. |
| Render normalny | Stroke, opcjonalnie fill dla closed path, opcjonalne markery. |
| Edycja | Punkty, handle, segmenty, insert/delete points. |
| Hit-test | Domyślnie po stroke z minimalną szerokością trafienia. |
| Sampling | Opcjonalnie expose sample points/tangents/normals dla node_graph i narzędzi. |

## Curve point schema

`points` nie może pozostać luźnym `json_array`. Implementacja wymaga formalnego schematu punktu.

| pole | typ | domyślnie | opis |
|---|---:|---:|---|
| id | id_ref | auto | Stabilny identyfikator punktu. |
| x | px | 0 | Pozycja X punktu. |
| y | px | 0 | Pozycja Y punktu. |
| in_handle_x | px/null | null | Lokalny lub absolutny handle wejściowy X. |
| in_handle_y | px/null | null | Lokalny lub absolutny handle wejściowy Y. |
| out_handle_x | px/null | null | Lokalny lub absolutny handle wyjściowy X. |
| out_handle_y | px/null | null | Lokalny lub absolutny handle wyjściowy Y. |
| point_type | enum | `corner` | `corner`, `smooth`, `bezier`, `auto`. |
| handle_mode | enum | `free` | `free`, `aligned`, `mirrored`, `auto`. |
| locked | boolean | false | Blokuje edycję punktu. |
| selected | boolean | false | Stan edytora, nie powinien być trwałym stanem runtime. |

## Parametry

| grupa | parametr | typ wartości | domyślnie | wymagane | zapis / runtime | opis / reguła |
|---|---|---:|---:|---:|---|---|
| base | schema_version | integer | 1 | yes | save | Wersja schematu do migracji. |
| identity | id | id_ref | auto | yes | save | Unikalny identyfikator instancji. |
| identity | name | string | `Curve` | yes | save | Nazwa w outlinerze/inspektorze. |
| identity | primitive_type | enum | `primitive.curve` | yes | save | Stały typ prymitywu. |
| identity | description | long_text | empty | no | save | Opis techniczny lub projektowy. |
| identity | notes | long_text | empty | no | save | Notatka użytkownika. |
| identity | tags | enum_multi | [] | no | save | Tagi do filtrowania i debugowania. |
| lifecycle | created_at | timestamp/null | null | no | save | Opcjonalny czas utworzenia. |
| lifecycle | updated_at | timestamp/null | null | no | save | Opcjonalny czas ostatniej zmiany. |
| lifecycle | source | enum | `manual` | yes | save | `manual`, `import`, `preset`, `node_graph`, `generated`. |
| hierarchy | parent_id | id_ref/null | null | no | save | Rodzic w drzewie sceny/UI. |
| hierarchy | layer_id | id_ref | default | yes | save | Warstwa logiczna. |
| hierarchy | z_index | integer | 0 | yes | save | Kolejność rysowania i hit-testu. |
| hierarchy | group_id | id_ref/null | null | no | save | Grupa logiczna. |
| transform | coordinate_space | enum | `parent` | yes | save | `parent`, `canvas`, `viewport`, `screen`, `normalized`. |
| transform | transform_order | enum | `translate_rotate_scale` | yes | save | Kolejność transformacji. |
| transform | x | px | 0 | yes | save | Lokalny offset X całej krzywej. |
| transform | y | px | 0 | yes | save | Lokalny offset Y całej krzywej. |
| transform | width | px/null | null | no | save | Opcjonalne explicit bounds X po normalizacji. |
| transform | height | px/null | null | no | save | Opcjonalne explicit bounds Y po normalizacji. |
| transform | rotation | angle_deg | 0 | yes | save | Obrót krzywej względem pivotu. |
| transform | scale_x | float | 1 | yes | save | Skala lokalna X. |
| transform | scale_y | float | 1 | yes | save | Skala lokalna Y. |
| transform | pivot_x | percent | 0.5 | yes | save | Pivot X względem bounds. |
| transform | pivot_y | percent | 0.5 | yes | save | Pivot Y względem bounds. |
| computed | world_bounds | rect/null | null | no | runtime | Bounds po transformacji. |
| computed | screen_bounds | rect/null | null | no | runtime | Bounds po kamerze/zoomie. |
| computed | length_cache | float/null | null | no | runtime | Długość krzywej po przeliczeniu. |
| computed | computed_snap_points | point_array | [] | no | runtime | Punkty snap po transformacji. |
| computed | sampled_points_cache | point_array | [] | no | runtime | Cache próbek, jeśli sampling aktywny. |
| geometry | path_source_mode | enum | `points` | yes | save | `points` albo `path_data`; decyduje o źródle prawdy. |
| geometry | curve_mode | enum | `polyline` | yes | save | `polyline`, `bezier`, `quadratic`, `catmull_rom`, `bspline`, `arc`. |
| geometry | points_coordinate_space | enum | `local` | yes | save | `local`, `normalized`, `canvas`. |
| geometry | points | curve_point_array | [] | yes | save | Lista formalnych `CurvePoint`. |
| geometry | path_data | string/null | null | no | save | Opcjonalny path string, jeśli import z SVG/path. |
| geometry | path_data_normalized | boolean | false | yes | save | Czy importowany path został przeliczony do lokalnych danych. |
| geometry | bounds_mode | enum | `auto_from_points` | yes | save | `auto_from_points`, `explicit_width_height`, `path_data_bounds`. |
| geometry | closed | boolean | false | yes | save | Czy krzywa zamyka się w pętlę. |
| geometry | fill_enabled | boolean | false | yes | save | Wypełnienie tylko dla zamkniętej ścieżki. |
| geometry | fill_color | color | `#101820` | yes | save | Kolor fill, jeśli fill_enabled. |
| geometry | fill_token | id_ref/null | null | no | save | Token fill. |
| geometry | closed_fill_policy | enum | `closed_only` | yes | save | `closed_only`, `force_close_for_fill`, `ignore_fill_when_open`. |
| geometry | winding_rule | enum | `nonzero` | yes | save | `nonzero`, `evenodd`. |
| geometry | resolution | integer | 16 | yes | save | Liczba próbek na segment krzywej. |
| geometry | tension | float | 0.5 | yes | save | Napięcie dla Catmull-Rom/typów interpolowanych. |
| geometry | simplify_enabled | boolean | false | yes | save | Czy uprościć punkty renderowane. |
| geometry | simplify_tolerance | px | 0.25 | yes | save | Tolerancja uproszczenia. |
| stroke | stroke_enabled | boolean | true | yes | save | Czy rysować stroke. |
| stroke | stroke_color | color | `#4a9bff` | yes | save | Kolor stroke, jeśli brak tokena. |
| stroke | stroke_token | id_ref/null | `color.accent.primary` | no | save | Token stroke. |
| stroke | stroke_width | px | 1 | yes | save | Grubość linii. |
| stroke | stroke_opacity | percent | 1 | yes | save | Przezroczystość stroke. |
| stroke | stroke_cap | enum | `round` | yes | save | `butt`, `round`, `square`. |
| stroke | stroke_join | enum | `round` | yes | save | `miter`, `round`, `bevel`. |
| stroke | stroke_miter_limit | float | 4 | yes | save | Limit miter join. |
| stroke | stroke_dash | string/null | null | no | save | Wzór linii przerywanej, np. `6 4`. |
| stroke | stroke_dash_offset | px | 0 | yes | save | Przesunięcie dash pattern. |
| marker | start_marker | enum/null | null | no | save | Marker początku: `arrow`, `dot`, `square`, `none`. |
| marker | end_marker | enum/null | null | no | save | Marker końca: `arrow`, `dot`, `square`, `none`. |
| marker | marker_size | px | 8 | yes | save | Rozmiar markerów końcowych. |
| marker | marker_token | id_ref/null | null | no | save | Token stylu markerów. |
| editing | editable_points | boolean | true | yes | save | Czy punkty są edytowalne. |
| editing | editable_segments | boolean | true | yes | save | Czy segmenty są edytowalne. |
| editing | show_handles | boolean | true | yes | save | Czy pokazywać uchwyty Béziera. |
| editing | point_snap_enabled | boolean | true | yes | save | Czy punkty krzywej snapują do grid/guides. |
| editing | handle_lock_mode | enum | `free` | yes | save | Globalny fallback: `free`, `aligned`, `mirrored`, `auto`. |
| editing | insert_point_enabled | boolean | true | yes | save | Czy można dodawać punkty na segmencie. |
| editing | delete_point_enabled | boolean | true | yes | save | Czy można usuwać punkty. |
| connector | connector_start_target_id | id_ref/null | null | no | save | Początkowy target, jeśli curve działa jako connector. |
| connector | connector_end_target_id | id_ref/null | null | no | save | Końcowy target, jeśli curve działa jako connector. |
| connector | connector_routing_mode | enum | `free` | yes | save | `free`, `straight`, `bezier`, `orthogonal`, `auto`. |
| behavior | enabled | boolean | true | yes | save | Czy element działa w runtime. |
| behavior | visible | boolean | true | yes | save | Czy jest renderowany. |
| behavior | locked | boolean | false | yes | save | Blokuje edycję. |
| behavior | selectable | boolean | true | yes | save | Czy można zaznaczyć. |
| behavior | hit_test_mode | enum | `stroke` | yes | save | `none`, `bounds`, `stroke`, `fill`, `points`, `debug_only`. |
| behavior | hit_stroke_width | px | 8 | yes | save | Minimalna szerokość trafienia kursorem. |
| behavior | hit_test_precision | px | 0.5 | yes | save | Dokładność trafiania w curve. |
| behavior | receives_events | boolean | true | yes | save | Czy może odbierać eventy. |
| behavior | emits_events | boolean | false | yes | save | Czy może emitować eventy. |
| behavior | cursor_on_hover | enum/null | null | no | save | Kursor edytora przy hover. |
| snapping | snap_enabled | boolean | true | yes | save | Czy krzywa jest kandydatem snapowania. |
| snapping | snap_points | enum_multi | [`points`,`bounds`,`center`] | yes | save | Punkty snapowania. |
| snapping | snap_priority | integer | 0 | yes | save | Priorytet snapowania. |
| sampling | expose_sample_points | boolean | false | yes | save | Czy udostępnić próbki innym narzędziom/node_graph. |
| sampling | sample_count | integer | 32 | yes | save | Liczba próbek, jeśli expose_sample_points. |
| sampling | expose_tangents | boolean | false | yes | save | Czy udostępnić tangenty. |
| sampling | expose_normals | boolean | false | yes | save | Czy udostępnić normale 2D. |
| layout | layout_role | enum | `shape` | yes | save | `shape`, `connector`, `guide`, `path`, `debug_line`, `motion_path`. |
| state | states | enum_multi | [`default`] | yes | runtime/save | Multi-state: `default`, `hover`, `selected`, `editing`, `disabled`, `error`, `warning`, `dirty`, `hidden`. |
| state | primary_state | enum | `default` | yes | runtime | Dominujący stan wizualny. |
| state | state_overrides | json_object | {} | no | runtime/save | Nadpisania stroke/fill/opacity/handles zależne od stanu. |
| debug | debug_show_points | boolean | false | yes | save | Pokazuje punkty. |
| debug | debug_show_handles | boolean | false | yes | save | Pokazuje handle. |
| debug | debug_show_tangents | boolean | false | yes | save | Pokazuje tangenty. |
| debug | debug_show_bounds | boolean | false | yes | save | Pokazuje bounds. |
| validation | require_points | boolean | true | yes | save | Curve wymaga przynajmniej 2 punktów, jeśli `path_source_mode = points`. |
| validation | allow_self_intersection | boolean | true | yes | save | Czy zamknięta ścieżka może się przecinać. |
| validation | validate_path_source | boolean | true | yes | save | Wymusza zgodność `path_source_mode`, `points`, `path_data`. |
| persistence | save_in_project | boolean | true | yes | save | Czy zapisuje się w projekcie. |
| persistence | migration_notes | string/null | null | no | docs | Notatka migracyjna. |
| metadata | metadata | json_object | {} | no | save | Dane dodatkowe bez wpływu na renderer. |
| metadata | custom_props | json_object | {} | no | save | Rozszerzenia aplikacji na engine. |
| editor | editor_flags | json_object | {} | no | editor | `expanded`, `pinned`, `inspector_hidden`, etc. |
| runtime | dirty_flags | enum_multi | [] | no | runtime | `transform`, `geometry`, `stroke`, `fill`, `markers`, `sampling`, `hit_test`, `debug`. |

## Implementation contract

| obszar | kontrakt |
|---|---|
| TypeScript | `CurvePrimitive extends PrimitiveBase`; `CurvePoint` jako osobny typ. |
| Required fields | `schema_version`, `id`, `primitive_type`, `name`, transform, `path_source_mode`, `curve_mode`, states. |
| Optional fields | `path_data`, markers, connector target ids, sampling outputs, custom props. |
| Computed fields | `world_bounds`, `screen_bounds`, `length_cache`, `computed_snap_points`, `sampled_points_cache`. |
| Renderer | Buduje path z `points` albo `path_data`, rysuje stroke, fill dla closed path, markery. |
| Hit-test | `stroke` testuje dystans do ścieżki z `hit_stroke_width`; `points` testuje punkty i handles. |
| Editing | Edytor punktów używa `CurvePoint`, handle modes, insert/delete point flags. |
| Snap | Points, bounds i center po transformacji; opcjonalnie sampled points. |
| Sampling | Cache próbek/tangentów/normali jest runtime-only i odświeżany przez dirty flags. |
| Validation | Jeśli `path_source_mode = points`, wymagaj min. 2 punktów. Jeśli `path_source_mode = path_data`, wymagaj `path_data`. |
| Serialization | Zapisywać source path/points, nie zapisywać cache. |
| Migration | `schema_version` steruje zmianami `CurvePoint`, path importu i markerów. |

## Relacje z innymi systemami

| system | relacja |
|---|---|
| `renderer` | Zamienia points/path_data na ścieżkę renderowalną. |
| `curve_editor` | Edytuje `CurvePoint`, handle modes, segment insertion. |
| `snap_engine` | Używa punktów, bounds, center, opcjonalnie próbek. |
| `node_graph` | Może używać curve jako path source, connector, guide, motion path. |
| `connector_runtime` | Używa target ids i routing mode. |
| `debug_overlay` | Pokazuje points, handles, tangents, bounds. |

## Zasada

`primitive.curve` opisuje przebieg i styl linii/ścieżki. Nie powinien zastępować rectangle tylko dlatego, że można narysować prostokąt z czterech punktów. Jeśli kształt ma być prostokątnym panelem, kartą, tłem albo buttonem, użyj `primitive.rectangle`.
