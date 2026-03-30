---
name: syncfusion-angular-list-box
description: Implement the Syncfusion Angular ListBox component for selection and data display. Use this when building interactive list interfaces with single or multiple selection modes. This skill covers drag-and-drop functionality, grouped data display, icons and templates, filtering, and accessibility support for enterprise-grade list components in Angular applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Implementing Syncfusion Angular ListBox

The ListBox component enables users to select one or more items from a predefined list with support for data binding, templates, drag-and-drop, sorting, grouping, and comprehensive accessibility features.


## Component Overview

The Syncfusion ListBox is a high-performance dropdown list replacement with advanced features:

| Feature | Benefit |
|---------|---------|
| **Multiple Selection Modes** | Single, multiple, or checkbox selection |
| **Data Binding** | Local arrays, complex objects, or remote services |
| **Drag & Drop** | Reorder items or transfer between lists |
| **Customization** | Icons, templates, grouping, sorting |
| **Accessibility** | WCAG 2.2, screen readers, keyboard navigation |
| **Performance** | Efficient rendering with large datasets |

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- Angular standalone vs NgModule patterns
- Basic component initialization
- CSS imports and theme selection
- Binding local data sources
- Running and testing the application

### Selection Modes and Interactions
📄 **Read:** [references/selection-modes.md](references/selection-modes.md)
- Single item selection
- Multiple item selection with SHIFT/CTRL
- Checkbox selection mode
- Select all functionality
- Handling change events
- Getting selected items programmatically

### Data Binding and Field Mapping
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding array of strings
- Binding array of objects
- Binding complex nested objects
- Remote data with DataManager and Query
- Field mapping (text, value, groupBy, iconCss)
- Troubleshooting data binding issues

### Drag-and-Drop Features
📄 **Read:** [references/drag-and-drop-features.md](references/drag-and-drop-features.md)
- Single ListBox drag-and-drop reordering
- Dual ListBox drag-and-drop transfer
- Drag and drop events (dragStart, drag, drop)
- Scope configuration for multiple lists
- Event handling and item manipulation
- Real-world dual ListBox patterns

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)
- Icons and iconCss field mapping
- Custom item templates
- Grouping items by category
- Sorting items (ascending/descending)
- CSS styling and theming
- Theme Studio integration
- Responsive design

### Practical Implementation Examples
📄 **Read:** [references/practical-examples.md](references/practical-examples.md)
- Filtering ListBox items
- Form submission with selected items
- Enable/disable items conditionally
- Scroller for large datasets
- Real-world use cases
- Common troubleshooting scenarios

### How-To Guides and Common Tasks
📄 **Read:** [references/how-to-guides.md](references/how-to-guides.md)
- Add items programmatically
- Select items programmatically
- Enable or disable items dynamically
- Enable scroller for large datasets
- Filter ListBox data with input
- Submit selected items in forms

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- All component properties with types, defaults, and examples
- All public methods with parameter signatures and usage examples
- All events with event argument interfaces and handler patterns
- Interface definitions: `FieldSettingsModel`, `SelectionSettingsModel`, `ToolbarSettingsModel`
- Event arg interfaces: `ListBoxChangeEventArgs`, `DragEventArgs`, `DropEventArgs`, `BeforeItemRenderEventArgs`, `FilteringEventArgs`, `SourceDestinationModel`
- Enum definitions: `SelectionMode`, `SortOrder`, `FilterType`, `ToolBarPosition`, `CheckBoxPosition`

## Quick Start Example

### Basic ListBox with Local Data

```typescript
import { Component, OnInit } from '@angular/core';
import { ListBoxModule } from '@syncfusion/ej2-angular-dropdowns';

@Component({
  imports: [ListBoxModule],
  standalone: true,
  selector: 'app-listbox',
  template: `
    <ejs-listbox 
      [dataSource]="items"
      [selectionSettings]="{ mode: 'Multiple' }">
    </ejs-listbox>
  `
})
export class ListBoxComponent implements OnInit {
  public items: { [key: string]: Object }[] = [];

  ngOnInit(): void {
    this.items = [
      { text: 'Option 1', id: '1' },
      { text: 'Option 2', id: '2' },
      { text: 'Option 3', id: '3' }
    ];
  }
}
```

### With Styles

```css
/* In your global styles.css */
@import '@syncfusion/ej2-base/styles/material3.css';
@import '@syncfusion/ej2-dropdowns/styles/material3.css';
@import '@syncfusion/ej2-inputs/styles/material3.css';
@import '@syncfusion/ej2-lists/styles/material3.css';
```

## Common Patterns

### Selection with Change Event

```typescript
import { ListBoxChangeEventArgs } from '@syncfusion/ej2-dropdowns';

selectedValues: string[] = [];

onSelectionChange(args: ListBoxChangeEventArgs): void {
  this.selectedValues = args.value as string[];
  console.log('Selected values:', args.value);
  console.log('Selected items:', args.items);
}
```

Template:
```html
<ejs-listbox
  [dataSource]="items"
  (change)="onSelectionChange($event)">
</ejs-listbox>
```

### Dual ListBox for Transfer

```typescript
sourceItems = [
  { text: 'Available Item 1', id: '1' },
  { text: 'Available Item 2', id: '2' }
];

selectedItems = [];
```

Template:
```html
<div style="display: flex; gap: 20px;">
  <div>
    <h4>Available Items</h4>
    <ejs-listbox
      [dataSource]="sourceItems"
      allowDragAndDrop="true"
      scope="transfer-list">
    </ejs-listbox>
  </div>
  
  <div>
    <h4>Selected Items</h4>
    <ejs-listbox
      [dataSource]="selectedItems"
      allowDragAndDrop="true"
      scope="transfer-list">
    </ejs-listbox>
  </div>
</div>
```

### Filtered ListBox

```typescript
allItems = [
  { text: 'Apple', category: 'Fruit' },
  { text: 'Carrot', category: 'Vegetable' },
  { text: 'Banana', category: 'Fruit' }
];

get filteredItems() {
  return this.allItems.filter(item => 
    item.text.toLowerCase().includes(this.searchTerm.toLowerCase())
  );
}
```

## Key Props

### Selection Configuration
- **`selectionSettings`**: `{ mode: 'Single' | 'Multiple', showCheckbox: boolean, showSelectAll: boolean, checkboxPosition: 'Left' | 'Right' }`  > **Note:** When using `showCheckbox: true`, you must inject `CheckBoxSelection`:
  > ```typescript
  > import { ListBoxComponent, CheckBoxSelection, ListBoxModule } from '@syncfusion/ej2-angular-dropdowns';
  > ListBoxComponent.Inject(CheckBoxSelection);
  > @Component({ imports: [ListBoxModule] })
  > ```- **`maximumSelectionLength`**: Limit how many items can be selected
- **`value`**: Get or set selected values (`string[] | number[] | boolean[]`)

### Data and Display
- **`dataSource`**: Array or DataManager with items
- **`fields`**: `{ text, value, groupBy, iconCss, disabled, htmlAttributes }`
- **`itemTemplate`**: Template for rendering each list item
- **`noRecordsTemplate`**: Template shown when no items match

### Interactions
- **`allowDragAndDrop`**: Enable drag-and-drop reordering/transfer
- **`scope`**: Identify related ListBoxes for drag-drop transfer
- **`allowFiltering`**: Show built-in filter search bar
- **`filterBarPlaceholder`**: Placeholder text for the filter bar
- **`filterType`**: `'StartsWith' | 'EndsWith' | 'Contains'`
- **`toolbarSettings`**: Configure toolbar buttons for dual-ListBox transfer

### Appearance
- **`height`**: Height of the ListBox
- **`sortOrder`**: `'None' | 'Ascending' | 'Descending'`
- **`enabled`**: Enable/disable component
- **`enableRtl`**: Right-to-left rendering
- **`cssClass`**: Additional CSS class

## Next Steps

1. **Get Started**: Read [references/getting-started.md](references/getting-started.md) for setup instructions
2. **Choose Selection Mode**: Review [references/selection-modes.md](references/selection-modes.md) for your use case
3. **Bind Data**: See [references/data-binding.md](references/data-binding.md) for data source options
4. **Add Interactions**: Explore [references/drag-and-drop-features.md](references/drag-and-drop-features.md) for advanced features
5. **Customize**: Check [references/customization.md](references/customization.md) for styling options
6. **See Examples**: Review [references/practical-examples.md](references/practical-examples.md) for real-world implementations
7. **API Reference**: Consult [references/api.md](references/api.md) for the complete properties, methods, events, and interface definitions

## Common Use Cases

- **Selection Forms**: Multi-select dropdown replacement
- **Transfer Lists**: Move items between lists (dual ListBox)
- **Categories**: Group-based item organization
- **Filtering**: Filter large datasets
- **Data Display**: Show structured list data
- **Accessibility**: Compliant selection interfaces
