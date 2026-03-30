---
name: syncfusion-angular-autocomplete
description: Implement the Syncfusion Angular AutoComplete component for type-ahead text suggestions with data binding, filtering, grouping, templates, virtualization, and form integration. Covers local and remote data sources, autofill, highlight search, custom filtering, keyboard navigation, and accessibility features. Use this when adding AutoComplete to Angular applications, configuring filtering behaviors, implementing data binding scenarios, or validating forms with autocomplete.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Implementing Syncfusion Angular AutoComplete Component

## Component Overview & Architecture

The **AutoComplete** is a text input component that provides matching suggestions as the user types. It is designed for:

1. **Type-ahead suggestions** — shows matching items from a data source as the user types
2. **Free-form input** — users can type any value, not restricted to the list
3. **Search/filter** — multiple filter strategies (StartsWith, Contains, EndsWith)
4. **Autofill** — automatically completes the first matched suggestion in the input
5. **Grouped suggestions** — categorize items by a `groupBy` field
6. **Virtual scrolling** — efficiently handles thousands of items
7. **Template customization** — item, group, header, footer, and empty-state templates

### Key Characteristics

| Aspect | Details |
|--------|---------|
| **Selection** | Single item; user can also type any free-form value |
| **Data Sources** | Local arrays, remote DataManager, OData, Web API, Observable (async pipe) |
| **Filtering** | Built-in filtering: `StartsWith`, `EndsWith`, `Contains` |
| **Autofill** | Completes the first match inline as the user types |
| **Performance** | Virtual scrolling for large datasets (1,000+ items) |
| **Forms** | Template-driven (`ngModel`) and reactive (`FormControl`) form integration |
| **Accessibility** | WCAG 2.2 compliant, full keyboard navigation, ARIA attributes |
| **Customization** | Item, group, header, footer, noRecords, actionFailure templates; CSS theming |

---

## Documentation Navigation Guide

### 📄 Getting Started
**Read:** [references/getting-started.md](references/getting-started.md)
- Install `@syncfusion/ej2-angular-dropdowns` package
- Set up Angular 21+ project with standalone components
- Import `AutoCompleteModule` and required CSS themes
- Create your first AutoComplete with basic data binding
- Configure popup height, width, and placeholder
- Enable two-way binding with `[(value)]`

### 📄 Data Binding
**Read:** [references/data-binding.md](references/data-binding.md)
- Bind to local arrays (strings, numbers, objects, complex objects)
- Map `value`, `text`, and `iconCss` fields via `fields` property
- Remote data using DataManager with OData, Web API adapters
- Async pipe pattern for RxJS Observables
- Object binding with `allowObjectBinding`
- Preselecting values using the `value` property

### 📄 Filtering & Search
**Read:** [references/filtering-and-search.md](references/filtering-and-search.md)
- Configure `filterType` (StartsWith, Contains, EndsWith)
- Limit suggestion count with `suggestionCount`
- Minimum character threshold with `minLength`
- Case-sensitive filtering with `ignoreCase`
- Diacritics/accent-insensitive filtering with `ignoreAccent`
- Debounce delay to optimize remote filtering with `debounceDelay`
- Custom filtering via the `filtering` event with `updateData`

### 📄 Grouping & Templates
**Read:** [references/grouping-and-templates.md](references/grouping-and-templates.md)
- Group suggestions by category using `fields.groupBy`
- Item templates for custom rendering
- Group header templates (inline and fixed)
- Header and footer templates for the popup
- Empty state with `noRecordsTemplate`
- Action failure template for remote data errors

### 📄 Feature Configuration
**Read:** [references/feature-configuration.md](references/feature-configuration.md)
- Autofill: inline suggestion completion with `autofill` property
- Highlight matched characters with `highlight` property
- Disable individual items with `fields.disabled` or `disableItem` method
- Disable entire component with `enabled` property
- Resizable popup with `allowResize`
- Virtual scrolling for large datasets with `enableVirtualization`
- Show/hide popup button with `showPopupButton`
- Show/hide clear button with `showClearButton`
- RTL support with `enableRtl`
- Sort order with `sortOrder`

### 📄 Form Support & Validation
**Read:** [references/form-support-and-validation.md](references/form-support-and-validation.md)
- Template-driven forms using `ngModel` and `FormsModule`
- Reactive forms using `FormControl`, `FormGroup`, and `ReactiveFormsModule`
- Binding and validation patterns
- Pre-selecting values via form model

### 📄 Accessibility & Localization
**Read:** [references/accessibility-and-localization.md](references/accessibility-and-localization.md)
- WCAG 2.2, Section 508, and ADA compliance
- Full keyboard shortcuts (Arrow keys, Tab, Enter, Escape, Alt+Down/Up)
- ARIA attributes: `aria-haspopup`, `aria-expanded`, `aria-selected`, `aria-autocomplete`
- Screen reader support and focus management
- Localization with `L10n.load()` for `noRecordsTemplate` and `actionFailureTemplate`
- RTL language support

### 📄 Advanced Patterns & How-To
**Read:** [references/advanced-patterns-how-to.md](references/advanced-patterns-how-to.md)
- Autofill feature with the `autofill` property
- Highlight searched characters with the `highlight` property
- Multi-field custom filtering with `Predicate` (filter by both Name and Code)
- Icon support via `fields.iconCss`
- Suggestion list on focus from browser local storage
- Custom search and highlight styling

### 📄 API Reference
**Read:** [references/api.md](references/api.md)
- Complete properties reference with types, defaults, and descriptions
- All methods with signatures and usage
- All events with payload types and handler examples

---

## Quick Start Example

```typescript
// app.component.ts (Angular 21 Standalone)
import { Component } from '@angular/core';
import { AutoCompleteModule } from '@syncfusion/ej2-angular-dropdowns';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [AutoCompleteModule],
  template: `
    <ejs-autocomplete
      id="sports"
      [dataSource]="sportsData"
      placeholder="Find a sport"
      [(value)]="selectedValue">
    </ejs-autocomplete>
  `
})
export class AppComponent {
  public sportsData: string[] = [
    'Badminton', 'Basketball', 'Cricket',
    'Football', 'Golf', 'Hockey', 'Tennis'
  ];
  public selectedValue: string = '';
}
```

**Install the package:**
```bash
ng add @syncfusion/ej2-angular-dropdowns
```

**Add CSS (styles.css):**
```css
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-inputs/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-dropdowns/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-lists/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material3.css';
```

---

## Common Patterns

### Pattern 1: Filter with Complex Object Data

```typescript
// Map fields for object array
public fields: Object = { value: 'Game' };
public sportsData: { [key: string]: Object }[] = [
  { Id: 'Game1', Game: 'Badminton' },
  { Id: 'Game2', Game: 'Cricket' }
];
```

```html
<ejs-autocomplete [dataSource]="sportsData" [fields]="fields" placeholder="Find a game">
</ejs-autocomplete>
```

**When to use:** Whenever your data is an array of objects and you need to display one field as the suggestion text.

---

### Pattern 2: Autofill + Highlight

```html
<ejs-autocomplete
  [dataSource]="data"
  [autofill]="true"
  [highlight]="true"
  filterType="StartsWith">
</ejs-autocomplete>
```

**When to use:** Search bars where users expect inline completion and visual emphasis on matched text.

---

### Pattern 3: Remote Data with Debounce

```typescript
import { DataManager, ODataV4Adaptor, Query } from '@syncfusion/ej2-data';

public data: DataManager = new DataManager({
  url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Customers',
  adaptor: new ODataV4Adaptor,
  crossDomain: true
});
public fields: Object = { value: 'ContactName' };
public query: Query = new Query().select(['ContactName']).take(6);
public debounceDelay: number = 500;
```

```html
<ejs-autocomplete
  [dataSource]="data"
  [fields]="fields"
  [query]="query"
  [debounceDelay]="debounceDelay"
  filterType="StartsWith">
</ejs-autocomplete>
```

**When to use:** API-backed autocomplete where you want to reduce request frequency.

---

### Pattern 4: Virtual Scrolling for Large Datasets

```typescript
import { AutoCompleteComponent, VirtualScroll } from '@syncfusion/ej2-angular-dropdowns';
AutoCompleteComponent.Inject(VirtualScroll);
```

```html
<ejs-autocomplete
  [dataSource]="records"
  [fields]="fields"
  [enableVirtualization]="true"
  popupHeight="200px">
</ejs-autocomplete>
```

**When to use:** Datasets with 1,000+ items where rendering all DOM elements at once is costly.

---

## Key Properties Quick Reference

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `dataSource` | Array \| DataManager | `[]` | Source data for suggestions |
| `fields` | FieldSettingsModel | `{ value: null }` | Map data columns to component |
| `placeholder` | string | `null` | Hint text when empty |
| `value` | string \| number \| boolean \| object | `null` | Selected/typed value |
| `filterType` | FilterType | `'Contains'` | How suggestions are matched |
| `minLength` | number | `1` | Minimum chars to trigger suggestions |
| `suggestionCount` | number | `20` | Max suggestions shown |
| `autofill` | boolean | `false` | Inline completion of first match |
| `highlight` | boolean | `false` | Highlight matched characters |
| `ignoreCase` | boolean | `true` | Case-insensitive filtering |
| `ignoreAccent` | boolean | — | Ignore diacritics in filtering |
| `debounceDelay` | number | `300` | Delay (ms) before filtering fires |
| `enableVirtualization` | boolean | `false` | Virtual scroll for large data |
| `allowResize` | boolean | `false` | Resizable popup |
| `showClearButton` | boolean | `true` | Show ✕ to clear value |
| `showPopupButton` | boolean | `false` | Show dropdown toggle button |
| `enabled` | boolean | `true` | Enable/disable entire component |
| `readonly` | boolean | `false` | Prevent user edits |
| `allowObjectBinding` | boolean | `false` | Bind value as full object |
| `sortOrder` | SortOrder | `null` | Sort suggestions (Ascending/Descending) |
| `popupHeight` | string \| number | `'300px'` | Popup list height |
| `popupWidth` | string \| number | `'100%'` | Popup list width |
| `locale` | string | `'en-US'` | Localization culture |
| `enableRtl` | boolean | `false` | Right-to-left rendering |

---

## Workflow Decision Tree

```
Need to implement AutoComplete?
│
├─ What's your data source?
│   ├─ Local array → See Data Binding: "Array of string" or "Array of object"
│   └─ Remote API  → See Data Binding: "Bind to remote data"
│
├─ How should filtering work?
│   ├─ Default (Contains)   → No extra config needed
│   ├─ StartsWith           → filterType="StartsWith"
│   ├─ Custom multi-field   → See Advanced Patterns: "Custom filtering"
│   └─ Accent-insensitive   → [ignoreAccent]="true"
│
├─ Need autofill (inline completion)?
│   └─ YES → [autofill]="true" + filterType="StartsWith"
│
├─ Highlight matched text?
│   └─ YES → [highlight]="true"
│
├─ Large dataset (1,000+ items)?
│   └─ YES → [enableVirtualization]="true" + inject VirtualScroll
│
├─ Using inside a form?
│   ├─ Template-driven → See Form Support: "ngModel"
│   └─ Reactive        → See Form Support: "FormControl"
│
└─ Need accessibility or localization?
    └─ YES → See Accessibility & Localization reference
```
