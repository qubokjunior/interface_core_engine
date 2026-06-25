# 02 — ui_element_types

Status: concept reference  
Scope: qubok_interface_core / interface_core_engine  
Category: `ui_element_types`  
Canonical registry: `ui_element_types`  
Source reference: screenshot atlas `02 ui_element_types`

## 1. Cel

`ui_element_types` definiuje słownik klas elementów interfejsu przed utworzeniem konkretnych komponentów, instancji lub presetów. Typ odpowiada na pytanie: **czym jest element UI i jaką ma podstawową odpowiedzialność**.

Przykład rozróżnienia:

| Warstwa | Przykład | Znaczenie |
|---|---|---|
| `ui_element_type` | `type.button` | Ogólna klasa: klikalny element uruchamiający akcję. |
| `component` | `component.save_button` | Konkretny komponent używający typu `type.button`. |
| `instance` | `button_save_01` | Konkretna instancja na canvasie lub w panelu. |
| `command` | `cmd.file.save` | Akcja uruchamiana przez komponent lub skrót. |

Zasada: `button.save_workspace` nie jest typem. To komponent, preset albo instancja oparta o `type.button`.

## 2. Zgodność konceptu

Koncepcja ze screenshota jest poprawna i bardzo ważna dla developmentu. Ten rejestr powinien istnieć wcześnie, bo łączy:

- inspector i edycję właściwości,
- command palette i skróty,
- node_graph i akcje,
- panels i workbench layout,
- design tokens i stany wizualne,
- validation i debug views,
- persistence i import/export.

Najważniejsza decyzja: typ UI nie powinien zawierać twardego wyglądu. Typ opisuje zachowanie i strukturę, a wygląd pochodzi z `design_tokens`, `states` i ewentualnie `component_props`.

## 3. Kategorie typów UI

| Kategoria | Rola | Przykłady |
|---|---|---|
| `primitive` | Najmniejsze elementy wizualne bez złożonej logiki. | rectangle, text, icon, line, region. |
| `control` | Element edytujący wartość albo uruchamiający akcję. | button, slider, checkbox, input, color picker. |
| `component` | Złożony blok zbudowany z kilku typów. | property row, toolbar item, card, list row. |
| `container` | Element trzymający inne elementy. | panel, toolbar, group, tab stack. |
| `navigation` | Przełączanie widoku lub wyboru zakresu danych. | tabs, breadcrumb, tree, command palette result. |
| `window` | Warstwa okna nad resztą UI. | modal dialog, popup, tooltip, toast. |
| `layout` | Element zmieniający rozmieszczenie lub granice. | splitter, dock zone, resize boundary. |
| `system` | Element sterujący aplikacją lub wyszukiwaniem. | command palette, status bar item, debug overlay. |
| `feedback` | Element informujący o stanie, błędzie lub progresie. | warning row, validation badge, progress bar, toast. |

## 4. Bazowy słownik typów ze screenshota

| Name | Type ID | Category | Core function | Key props | Main states |
|---|---|---|---|---|---|
| Button | `type.button` | control | Execute command. | `label`, `icon`, `command_id`, `enabled`, `variant` | default, hover, pressed, disabled |
| Icon Button | `type.button.icon` | control | Execute compact command. | `icon`, `tooltip`, `command_id`, `active` | hover, pressed, active, disabled |
| Toggle Button | `type.button.toggle` | control | Toggle boolean state. | `checked`, `icon_on`, `icon_off`, `group_id` | off, on, hover, disabled |
| Checkbox | `type.checkbox` | control | Edit true/false. | `checked`, `label`, `indeterminate` | unchecked, checked, mixed |
| Radio Button | `type.radio` | control | Select enum option. | `value`, `selected_value`, `group_id` | selected, unselected |
| Switch | `type.switch` | control | Toggle persistent setting. | `checked`, `label`, `size` | off, on, disabled |
| Text Input | `type.input.text` | control | Edit short string. | `value`, `placeholder`, `validation` | focused, invalid, disabled |
| Number Input | `type.input.number` | control | Edit numeric value. | `value`, `min`, `max`, `step`, `unit` | focused, invalid |
| Search Input | `type.input.search` | control | Filter/search. | `query`, `debounce`, `clear_button` | typing, loading, no_results |
| Text Area | `type.input.textarea` | control | Edit long text. | `value`, `min_rows`, `max_rows`, `wrap` | focused, scroll |
| Dropdown | `type.dropdown` | control | Select one option. | `options`, `selected_value`, `searchable` | closed, open, selected |
| Multi Select | `type.multiselect` | control | Select many options/tags. | `selected_values`, `chips`, `max_items` | open, selected, invalid |
| Slider | `type.slider` | control | Edit scalar value. | `value`, `min`, `max`, `step`, `unit` | hover, dragging |
| Range Slider | `type.slider.range` | control | Edit min/max range. | `min_value`, `max_value`, `step` | dragging_min, dragging_max |
| Color Picker | `type.color_picker` | control | Edit color/token color. | `color`, `format`, `alpha`, `token_link` | open, invalid |
| File Picker | `type.file_picker` | control | Select file or folder path. | `path`, `mode`, `extensions`, `recent_paths` | valid, missing |
| Tabs | `type.tabs` | navigation | Switch visible content. | `active_tab`, `tabs`, `closable` | active, inactive, dragging |
| Toolbar | `type.toolbar` | container | Hold frequent actions. | `orientation`, `items`, `overflow` | compact, overflow |
| Panel | `type.panel` | container | Hold components. | `title`, `dock_region`, `min_size` | focused, collapsed |
| Property Row | `type.property_row` | component | Edit one property. | `label`, `editor`, `value`, `reset` | mixed, invalid |
| Modal Dialog | `type.dialog.modal` | window | Confirm/apply/cancel. | `title`, `body`, `actions` | open, loading, error |
| Splitter | `type.splitter` | layout | Resize panels. | `axis`, `min_before`, `min_after` | hover, dragging |
| Dock Zone | `type.dock_zone` | layout | Accept dock target. | `region`, `allowed_panels`, `snap_rule` | valid_target, invalid_target |
| Command Palette | `type.command_palette` | system | Search/execute commands. | `query`, `results`, `recent` | open, searching |

## 5. Brakujące typy, które warto dodać do słownika

Poniższe elementy odnoszą się bezpośrednio do `ui_element_types`, ale nie są jawnie wymienione na karcie. Warto je przewidzieć jako osobne typy lub podtypy, żeby później nie mieszać ich z komponentami.

| Name | Proposed Type ID | Category | Dlaczego potrzebne |
|---|---|---|---|
| Label | `type.label` | primitive | Stały tekst opisowy przy polach, sekcjach i kontrolkach. |
| Text Block | `type.text_block` | primitive | Dłuższy tekst informacyjny, opis panelu, hint. |
| Icon | `type.icon` | primitive | Ikona jako osobny element, nie tylko część buttona. |
| Divider | `type.divider` | primitive/layout | Linia dzieląca sekcje, listy i menu. |
| Spacer | `type.spacer` | layout | Kontrolowany pusty odstęp w layoutach. |
| Group | `type.group` | container | Kontener logiczny bez pełnego panelu. |
| Section | `type.section` | container | Sekcja inspectora/panelu z nagłówkiem i collapse. |
| Card | `type.card` | component/container | Mały blok informacji lub ustawień. |
| List | `type.list` | component | Lista wyników, komponentów, komend lub obiektów. |
| Table | `type.table` | component | Dane w kolumnach: tokens, commands, props, validation. |
| Tree | `type.tree` | component/navigation | Hierarchia obiektów, plików, layoutów. |
| Breadcrumb | `type.breadcrumb` | navigation | Ścieżka wewnątrz struktury paneli/projektu. |
| Context Menu | `type.menu.context` | window/system | Menu prawym kliknięciem albo z przycisku `...`. |
| Popup | `type.popup` | window | Lekka warstwa wyboru, mniejsza niż modal. |
| Tooltip | `type.tooltip` | feedback/window | Krótki opis narzędzia lub wartości. |
| Toast | `type.toast` | feedback/window | Krótki status: zapisano, błąd, ostrzeżenie. |
| Badge | `type.badge` | feedback | Licznik, warning, status lub marker walidacji. |
| Progress Bar | `type.progress` | feedback | Ładowanie, eksport, kompilacja, walidacja. |
| Spinner | `type.spinner` | feedback | Lekki stan loading. |
| Canvas | `type.canvas` | system/container | Główna przestrzeń edycji obiektów. |
| Selection Box | `type.selection_box` | system/feedback | Ramka zaznaczenia i multi-select. |
| Transform Handle | `type.transform_handle` | system/control | Punkty resize/rotate/scale na canvasie. |
| Debug Overlay | `type.debug_overlay` | system/feedback | Bounds, guides, ids, performance, validation. |

## 6. Minimalny model danych dla rekordu typu UI

```ts
export type UiElementTypeCategory =
  | 'primitive'
  | 'control'
  | 'component'
  | 'container'
  | 'navigation'
  | 'window'
  | 'layout'
  | 'system'
  | 'feedback';

export interface UiElementTypeDefinition {
  type_id: string;
  name: string;
  category: UiElementTypeCategory;
  description: string;
  core_function: string;
  allowed_children?: string[];
  key_props: string[];
  default_props?: Record<string, unknown>;
  supported_states: string[];
  default_state?: string;
  related_value_types?: string[];
  default_editor?: string;
  command_bindings?: string[];
  node_graph_support?: boolean;
  inspector_support?: boolean;
  persistence_support?: boolean;
  accessibility_role?: string;
  debug_metadata?: string[];
}
```

## 7. Reguły projektowe

1. Typ opisuje klasę zachowania, nie konkretny komponent.  
2. Każdy komponent wskazuje dokładnie jeden główny `type_id`.  
3. Typ może mieć podtypy, ale podtyp nie powinien zastępować komponentu.  
4. Typy nie przechowują twardych kolorów, fontów i spacingu; korzystają z `design_tokens`.  
5. Typy nie powinny przechowywać wartości biznesowych aplikacji.  
6. Typy mogą wskazywać obsługiwane stany, ale nie powinny same wykonywać logiki stanu.  
7. Typy kontenerowe muszą określać, jakie dzieci mogą zawierać.  
8. Typy layoutowe muszą mieć min/max oraz reguły kolizji/snap.  
9. Typy systemowe muszą być widoczne w debug tools, ale niekoniecznie w runtime app.  
10. Typy powinny być stabilne nazwowo, bo będą używane przez persistence, node_graph i command registry.

## 8. Relacje z innymi rejestrami

| Relacja | Znaczenie |
|---|---|
| `ui_element_types -> ui_components` | Komponent wskazuje, jakiego typu używa. |
| `ui_element_types -> ui_states` | Typ deklaruje stany, które potrafi reprezentować. |
| `ui_element_types -> design_tokens` | Typ dziedziczy wygląd z tokenów. |
| `ui_element_types -> value_types` | Typ może edytować lub wyświetlać określone wartości. |
| `ui_element_types -> control_editors` | Typ kontrolki może być edytorem wartości. |
| `ui_element_types -> commands` | Typy akcyjne uruchamiają komendy. |
| `ui_element_types -> panels` | Panel jest typem kontenerowym i jednocześnie częścią workbench layout. |
| `ui_element_types -> node_graph` | Typy mogą być źródłem zdarzeń, targetem akcji lub wizualną reprezentacją node. |
| `ui_element_types -> feedback_validation` | Typy muszą umieć pokazać błędy, ostrzeżenia i brakujące dane. |

## 9. Priorytet dla MVP

| Priorytet | Typy |
|---|---|
| P0 | button, icon button, input text, number input, checkbox, switch, slider, dropdown, panel, property row, tabs, splitter, dock zone, command palette |
| P1 | text area, search input, color picker, file picker, toolbar, table, tree, context menu, tooltip, toast, badge |
| P2 | multi select, range slider, modal dialog, progress bar, canvas overlay, transform handle, debug overlay |
| P3 | radial menu, plugin panel widgets, advanced macro editor widgets, custom node widgets |

## 10. Decyzja robocza

`ui_element_types` powinien stać się jednym z podstawowych plików dokumentacyjno-technicznych interface_core_engine. To jest słownik, który porządkuje, czy coś jest prymitywem, kontrolką, komponentem, panelem, oknem, layout itemem czy elementem systemowym. Bez tego inspektor, node_graph, command palette i panel builder będą szybko mieszać odpowiedzialności.

Najbliższy krok po tej karcie: opracować `value_types` i `control_editors`, ponieważ te dwa rejestry bezpośrednio zasilają automatyczny inspector i edycję propsów komponentów.
