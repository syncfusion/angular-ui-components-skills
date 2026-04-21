# Tooltips

Enable and customize interactive tooltips for 3D charts.

## Table of Contents

- [Overview](#overview)
  - [API Reference](#api-reference)
- [Enable Tooltips](#enable-tooltips)
  - [Basic Tooltip](#basic-tooltip)
  - [Disable Tooltips](#disable-tooltips)
- [Tooltip Formatting](#tooltip-formatting)
  - [Default Format](#default-format)
  - [Custom Format Patterns](#custom-format-patterns)
  - [Example Formats](#example-formats)
- [Tooltip Templates](#tooltip-templates)
  - [HTML Template](#html-template)
  - [Dynamic Template](#dynamic-template)
- [Tooltip Styling](#tooltip-styling)
  - [Background and Border](#background-and-border)
  - [Tooltip Offset](#tooltip-offset)
- [Tooltip Behavior](#tooltip-behavior)
  - [Shared Tooltip](#shared-tooltip)
  - [Persistent Tooltip](#persistent-tooltip)
  - [Show Delay](#show-delay)
- [Tooltip Events](#tooltip-events)
  - [Show Tooltip on Click](#show-tooltip-on-click)
- [Common Tooltip Patterns](#common-tooltip-patterns)
  - [Pattern 1: Currency Display](#pattern-1-currency-display)
  - [Pattern 2: Category and Value](#pattern-2-category-and-value)
  - [Pattern 3: Multi-Series Comparison](#pattern-3-multi-series-comparison)
  - [Pattern 4: Rich Information](#pattern-4-rich-information)
- [Accessibility Considerations](#accessibility-considerations)
- [Performance Tips](#performance-tips)

## Overview

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)

## Enable Tooltips

### Basic Tooltip

```typescript
import { Component } from '@angular/core';
import { Chart3DAllModule } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-tooltip',
  standalone: true,
  imports: [Chart3DAllModule],
  template: `
    <ejs-chart3d [tooltip]="tooltip">
      <e-chart3d-series-collection>
        <e-chart3d-series [dataSource]="data" xName="month" yName="sales" type="Column">
        </e-chart3d-series>
      </e-chart3d-series-collection>
    </ejs-chart3d>
  `
})
export class TooltipComponent {
  data = [
    { month: 'Jan', sales: 40 },
    { month: 'Feb', sales: 50 }
  ];

  tooltip = {
    enable: true
  };
}
```

### Disable Tooltips

```typescript
tooltip = {
  enable: false
};
```

## Tooltip Formatting

### Default Format

```typescript
tooltip = {
  enable: true,
  format: '${series.name} - ${point.x} : ${point.y}'
};
```

### Custom Format Patterns

| Pattern | Result |
|---------|--------|
| `${series.name}` | Series name |
| `${point.x}` | X-axis value |
| `${point.y}` | Y-axis value |
| `${point.text}` | Data point text |

### Example Formats

```typescript
// Simple value tooltip
tooltip = {
  enable: true,
  format: '${point.y}'
};

// Category and value
tooltip = {
  enable: true,
  format: '${point.x}: ${point.y}'
};

// Currency format
tooltip = {
  enable: true,
  format: '${point.x}: ${point.y}K'
};

// Multi-line format
tooltip = {
  enable: true,
  format: '<b>${point.x}</b><br>Sales: ${point.y}'
};
```

## Tooltip Templates

### HTML Template

Create rich-formatted tooltips:

```typescript
@Component({
  template: `
    <ejs-chart3d [tooltip]="tooltip" [primaryXAxis]='primaryXAxis'>
      <e-chart3d-series-collection>
        <e-chart3d-series [dataSource]="data" xName="month" yName="sales" type="Column">
        </e-chart3d-series>
      </e-chart3d-series-collection>
    </ejs-chart3d>
  `
})
export class TemplateTooltipComponent {

  primaryXAxis = {
        valueType: 'Category'
  }

  data = [
    { month: 'Jan', sales: 40 },
    { month: 'Feb', sales: 50 }
  ];

  tooltip = {
    enable: true,
    template: '<div><strong>${point.x}</strong><br>Sales: $${point.y}K</div>'
  };
}
```

### Dynamic Template

```typescript
tooltip = {
  enable: true,
  template: '<div style="background: #f0f0f0; padding: 8px; border-radius: 4px;">' +
            '<b>${point.x}</b><br>' +
            '<span style="color: #0066cc;">Revenue: $${point.y}K</span><br>' +
            '<span style="color: #999;">Click to see details</span>' +
            '</div>'
};
```

## Tooltip Styling

### Background and Border

```typescript
@Component({
  template: `
    <ejs-chart3d [tooltip]="tooltip" [primaryXAxis]='primaryXAxis'>
      <e-chart3d-series-collection>
        <e-chart3d-series [dataSource]="data" xName="x" yName="y" type="Column">
        </e-chart3d-series>
      </e-chart3d-series-collection>
    </ejs-chart3d>
  `
})
export class StyledTooltipComponent {

  primaryXAxis = {
        valueType: 'Category'
  }

  data = [{ x: 'A', y: 40 }];

  tooltip = {
    enable: true,
    fill: '#f9f9f9',
    border: { color: '#999', width: 1 },
    textStyle: {
      fontFamily: 'Arial',
      fontStyle: 'Normal',
      fontWeight: '400',
      size: '14px',
      color: '#333'
    }
  };
}
```

### Tooltip Offset

```typescript
tooltip = {
  enable: true,
  shared: false,
  duration: 200,
  offset: 10  // Distance from pointer
};
```

## Tooltip Behavior

### Shared Tooltip

Show one tooltip for all series at same X value:

```typescript
tooltip = {
  enable: true,
  shared: true
};
```

### Persistent Tooltip

```typescript
tooltip = {
  enable: true,
  duration: 3000  // Display for 3 seconds
};
```

### Show Delay

```typescript
tooltip = {
  enable: true,
  showDelay: 500  // Wait 500ms before showing
};
```

## Tooltip Events

### Show Tooltip on Click

```typescript
@Component({
  template: `
    <ejs-chart3d [tooltip]="tooltip" (pointRender)="onPointRender($event)" [primaryXAxis]='primaryXAxis'>
      <e-chart3d-series-collection>
        <e-chart3d-series [dataSource]="data" xName="x" yName="y" type="Column">
        </e-chart3d-series>
      </e-chart3d-series-collection>
    </ejs-chart3d>
  `
})
export class TooltipEventComponent {
  primaryXAxis = {
        valueType: 'Category'
  }

  data = [{ x: 'A', y: 40 }];

  tooltip = {
    enable: true
  };

  onPointRender(args: any) {
    // Customize tooltip on render
    if (args.data.y > 50) {
      args.tooltip.template = '<strong>High Value: ${point.y}</strong>';
    }
  }
}
```

## Common Tooltip Patterns

### Pattern 1: Currency Display

For financial data:

```typescript
tooltip = {
  enable: true,
  format: '<b>${point.x}</b><br>Amount: $${point.y}K'
};
```

### Pattern 2: Category and Value

For simple data display:

```typescript
tooltip = {
  enable: true,
  format: '${point.x}<br>Value: ${point.y}'
};
```

### Pattern 3: Multi-Series Comparison

Show all series values:

```typescript
tooltip = {
  enable: true,
  shared: true,
  format: '<b>${point.x}</b><br>${series.name}: ${point.y}'
};
```

### Pattern 4: Rich Information

Include additional context:

```typescript
tooltip = {
  enable: true,
  template: '<div style="padding: 10px; background: white; border-radius: 4px;">' +
            '<strong style="font-size: 14px;">${point.x}</strong><br>' +
            '<span style="color: #0066cc; font-size: 12px;">Sales: $${point.y}K</span><br>' +
            '<span style="color: #666; font-size: 11px;">Updated: Today</span>' +
            '</div>'
};
```

## Accessibility Considerations

Tooltips should:
- **Be readable:** Font size ≥ 12px
- **Have contrast:** 4.5:1 minimum ratio
- **Be dismissible:** ESC key or clicking away
- **Not obscure:** Don't cover critical data

## Performance Tips

- Avoid complex HTML in templates
- Use simple format strings
- Limit template rendering for large datasets
- Consider lazy loading tooltip data
