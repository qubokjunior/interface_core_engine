# Primitive: Curve

`primitive.curve` to podstawowy prymityw liniowy/ścieżkowy: linia, krzywa Béziera, polyline, ścieżka edytora curve, connector, guide, granica debug, motion path albo lekki shape oparty na punktach. Curve opisuje geometrię przez punkty, segmenty i styl stroke.

## Parametry

| grupa | parametr | typ wartości | domyślnie | opis / reguła |
|---|---|---:|---:|---|
| identity | id | id_ref | auto | Unikalny identyfikator instancji. |
| identity | name | string | `Curve` | Nazwa widoczna w outlinerze/inspektorze. |
| identity | primitive_type | enum | `primitive.curve` | Stały typ prymitywu. |
| identity | description | long_text | empty | Opcjonalna notatka projektowa. |
| identity | tags | enum_multi | [] | Tagi do filtrowania i debugowania. |
| hierarchy | parent_id | id_ref/null | null | Rodzic w drzewie sceny/UI. |
| hierarchy | layer_id | id_ref | default | Warstwa logiczna. |
| hierarchy | z_index | integer | 0 | Kolejność rysowania i hit-testu. |
| hierarchy | group_id | id_ref/null | null | Grupa logiczna. |
| transform | x | px | 0 | Lokalny offset X całej krzywej. |
| transform | y | px | 0 | Lokalny offset Y całej krzywej. |
| transform | width | px/null | null | Opcjonalne bounds X po normalizacji. |
| transform | height | px/null | null | Opcjonalne bounds Y po normalizacji. |
| transform | rotation | float_deg | 0 | Obrót krzywej względem pivotu. |
| transform | scale_x | float | 1 | Skala lokalna X. |
| transform | scale_y | float | 1 | Skala lokalna Y. |
| transform | pivot_x | percent | 0.5 | Pivot X względem bounds. |
| transform | pivot_y | percent | 0.5 | Pivot Y względem bounds. |
| geometry | curve_mode | enum | `polyline` | `polyline`, `bezier`, `quadratic`, `catmull_rom`, `bspline`, `arc`. |
| geometry | points | json_array | [] | Lista punktów `{x,y}` albo punktów z handle/tangent. |
| geometry | path_data | string/null | null | Opcjonalny path string, jeśli import z SVG/path. |
| geometry | closed | boolean | false | Czy krzywa zamyka się w pętlę. |
| geometry | fill_enabled | boolean | false | Wypełnienie tylko dla zamkniętej ścieżki. |
| geometry | fill_color | color | `#101820` | Kolor fill, jeśli fill_enabled. |
| geometry | fill_token | id_ref/null | null | Token fill. |
| geometry | winding_rule | enum | `nonzero` | `nonzero`, `evenodd`. |
| geometry | resolution | integer | 16 | Liczba próbek na segment krzywej. |
| geometry | tension | float | 0.5 | Napięcie dla Catmull-Rom/typów interpolowanych. |
| geometry | simplify_enabled | boolean | false | Czy uprościć punkty renderowane. |
| geometry | simplify_tolerance | px | 0.25 | Tolerancja uproszczenia. |
| stroke | stroke_enabled | boolean | true | Czy rysować stroke. |
| stroke | stroke_color | color | `#4a9bff` | Kolor stroke, jeśli brak tokena. |
| stroke | stroke_token | id_ref/null | `color.accent.primary` | Token stroke. |
| stroke | stroke_width | px | 1 | Grubość linii. |
| stroke | stroke_opacity | percent | 1 | Przezroczystość stroke. |
| stroke | stroke_cap | enum | `round` | `butt`, `round`, `square`. |
| stroke | stroke_join | enum | `round` | `miter`, `round`, `bevel`. |
| stroke | stroke_miter_limit | float | 4 | Limit miter join. |
| stroke | stroke_dash | string/null | null | Wzór linii przerywanej, np. `6 4`. |
| stroke | stroke_dash_offset | px | 0 | Przesunięcie dash pattern. |
| arrow | start_marker | enum/null | null | Marker początku: `arrow`, `dot`, `square`, `none`. |
| arrow | end_marker | enum/null | null | Marker końca: `arrow`, `dot`, `square`, `none`. |
| arrow | marker_size | px | 8 | Rozmiar markerów końcowych. |
| editing | editable_points | boolean | true | Czy punkty są edytowalne. |
| editing | show_handles | boolean | true | Czy pokazywać uchwyty Béziera. |
| editing | point_snap_enabled | boolean | true | Czy punkty krzywej snapują do grid/guides. |
| editing | handle_lock_mode | enum | `free` | `free`, `aligned`, `mirrored`, `auto`. |
| editing | insert_point_enabled | boolean | true | Czy można dodawać punkty na segmencie. |
| editing | delete_point_enabled | boolean | true | Czy można usuwać punkty. |
| behavior | enabled | boolean | true | Czy element działa w runtime. |
| behavior | visible | boolean | true | Czy jest renderowany. |
| behavior | locked | boolean | false | Blokuje edycję. |
| behavior | selectable | boolean | true | Czy można zaznaczyć. |
| behavior | hit_test_mode | enum | `stroke` | `none`, `bounds`, `stroke`, `fill`, `points`, `debug_only`. |
| behavior | hit_stroke_width | px | 8 | Minimalna szerokość trafienia kursorem. |
| snapping | snap_enabled | boolean | true | Czy krzywa jest kandydatem snapowania. |
| snapping | snap_points | enum_multi | [`points`,`bounds`,`center`] | Punkty snapowania. |
| sampling | expose_sample_points | boolean | false | Czy udostępnić próbki innym narzędziom/node_graph. |
| sampling | sample_count | integer | 32 | Liczba próbek, jeśli expose_sample_points. |
| sampling | expose_tangents | boolean | false | Czy udostępnić tangenty. |
| sampling | expose_normals | boolean | false | Czy udostępnić normale 2D. |
| layout | layout_role | enum | `shape` | `shape`, `connector`, `guide`, `path`, `debug_line`, `motion_path`. |
| state | state | enum | `default` | `default`, `hover`, `selected`, `editing`, `disabled`, `error`, `hidden`. |
| debug | debug_show_points | boolean | false | Pokazuje punkty. |
| debug | debug_show_handles | boolean | false | Pokazuje handle. |
| debug | debug_show_tangents | boolean | false | Pokazuje tangenty. |
| debug | debug_show_bounds | boolean | false | Pokazuje bounds. |
| validation | require_points | boolean | true | Curve wymaga przynajmniej 2 punktów, jeśli brak path_data. |
| validation | allow_self_intersection | boolean | true | Czy zamknięta ścieżka może się przecinać. |
| persistence | save_in_project | boolean | true | Czy zapisuje się w projekcie. |
| metadata | metadata | json_object | {} | Dane dodatkowe bez wpływu na renderer. |

## Zasada

`primitive.curve` opisuje przebieg i styl linii/ścieżki. Nie powinien zastępować rectangle tylko dlatego, że można narysować prostokąt z czterech punktów. Jeśli kształt ma być prostokątnym panelem, kartą, tłem albo buttonem, użyj `primitive.rectangle`.
