# Getting Started

## Installation & Dependencies

The Syncfusion Angular Diagram component requires the following packages:

```bash
npm install @syncfusion/ej2-angular-core
npm install @syncfusion/ej2-angular-diagrams
```

### Package Dependencies

```
@syncfusion/ej2-angular-diagrams
├── @syncfusion/ej2-base
├── @syncfusion/ej2-data
├── @syncfusion/ej2-navigations
├── @syncfusion/ej2-inputs
├── @syncfusion/ej2-popups
├── @syncfusion/ej2-buttons
├── @syncfusion/ej2-lists
└── @syncfusion/ej2-splitbuttons
```

## Theme Setup

### Step 1: Import CSS Theme

In your `main.ts` or `styles.css`:

```typescript
// main.ts
import '@syncfusion/ej2-angular-theme-default/styles/material.css';
```

**Available themes:**
- `material.css` - Material Design (recommended)
- `bootstrap5.css` - Bootstrap 5 theme
- `fabric.css` - Microsoft Fabric theme
- `tailwind.css` - Tailwind CSS theme
- `highcontrast.css` - High contrast for accessibility

### Step 2: Import Base Bundle

```typescript
import '@syncfusion/ej2-base';
```

## Basic Setup

### Using Standalone Components (Angular 14+)

```typescript
import { Component } from '@angular/core';
import { DiagramComponent, Inject } from '@syncfusion/ej2-angular-diagrams';

@Component({
  selector: 'app-diagram',
  template: '<ejs-diagram #diagram></ejs-diagram>',
  standalone: true,
  imports: [DiagramComponent],
  viewProviders: [Inject(DiagramComponent)]
})
export class DiagramComponent {}
```

### Using NgModule (Angular <14)

```typescript
import { NgModule } from '@angular/core';
import { DiagramModule } from '@syncfusion/ej2-angular-diagrams';

@NgModule({
  imports: [DiagramModule],
  declarations: [AppComponent]
})
export class AppModule {}
```

## Module Injection (Inject Directive)

Syncfusion uses **opt-in feature loading** via the Inject directive:

```typescript
import { 
  DiagramComponent, 
  Inject, 
  BpmnDiagrams,
  SymbolPalette,
  HierarchicalTree
} from '@syncfusion/ej2-angular-diagrams';

@Component({
  selector: 'app-diagram',
  template: `<ejs-diagram #diagram></ejs-diagram>`,
  standalone: true,
  imports: [DiagramComponent],
  viewProviders: [
    Inject(BpmnDiagrams),
    Inject(SymbolPalette),
    Inject(HierarchicalTree)
  ]
})
export class DiagramComponent {}
```

**Why Inject?**
- Small bundle (only inject what you use)
- Clear feature dependencies in code
- Prevents unused code inclusion

### Common Feature Modules

| Module | Purpose |
|--------|---------|
| `BpmnDiagrams` | BPMN shapes and notation |
| `Swimlane` | Swimlane support |
| `SymbolPalette` | Draggable symbol library |
| `HierarchicalTree` | Hierarchical auto-layout |
| `OrganizationalChart` | Org-chart layout |
| `MindMap` | Mindmap layout |
| `RadialTree` | Radial layout |
| `ComplexHierarchicalTree` | Complex hierarchical layout |

## Basic Diagram Component

### Minimal Example

```typescript
import { Component } from '@angular/core';
import { DiagramComponent, Inject } from '@syncfusion/ej2-angular-diagrams';

@Component({
  selector: 'app-root',
  template: `
    <ejs-diagram #diagram
      [width]="'100%'"
      [height]="'600px'"
      [nodes]="nodes"
      [connectors]="connectors">
    </ejs-diagram>
  `,
  standalone: true,
  imports: [DiagramComponent],
  viewProviders: [Inject(DiagramComponent)]
})
export class AppComponent {
  nodes = [
    { id: 'node1', width: 100, height: 100, offsetX: 100, offsetY: 100 },
    { id: 'node2', width: 100, height: 100, offsetX: 300, offsetY: 100 }
  ];

  connectors = [
    { id: 'connector1', sourceID: 'node1', targetID: 'node2' }
  ];
}
```

## Using the Diagram Component in Templates

Add the diagram component to your Angular templates:

```html
<ejs-diagram #diagram
  [width]="'100%'"
  [height]="'600px'"
  [nodes]="nodes"
  [connectors]="connectors">
</ejs-diagram>
```

## CSS Imports Summary

```typescript
// main.ts
import '@syncfusion/ej2-base';
import '@syncfusion/ej2-angular-theme-default/styles/material.css';
```

## Verification

To verify setup is working:

```typescript
@Component({
  selector: 'app-test',
  template: `
    <ejs-diagram #diagram 
      [width]="'400px'" 
      [height]="'300px'"
      [nodes]="[{ id: 'test', offsetX: 100, offsetY: 100, width: 80, height: 80 }]">
    </ejs-diagram>
  `,
  standalone: true,
  imports: [DiagramComponent],
  viewProviders: [Inject(DiagramComponent)]
})
export class TestComponent {}
```

If a white diagram canvas appears, setup is successful. Now proceed to [nodes.md](nodes.md) to add content.
