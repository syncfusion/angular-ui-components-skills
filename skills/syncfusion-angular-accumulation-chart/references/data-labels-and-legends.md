# Data Labels and Legends

## Table of Contents
- [Data Labels Overview](#data-labels-overview)
- [Label Positioning](#label-positioning)
- [Label Formatting](#label-formatting)
- [Label Templates](#label-templates)
- [Legends](#legends)
- [Legend Customization](#legend-customization)
- [Legend Events](#legend-events)
- [Combining Labels and Legends](#combining-labels-and-legends)

## Data Labels Overview

Data labels display information about each data point directly on the chart. They can show values, percentages, categories, or custom formatted text.

### Enable Data Labels

```typescript
<e-accumulation-series
  [dataSource]="data"
  xName="x"
  yName="y"
  type="Pie"
  [dataLabel]="{ visible: true }">
</e-accumulation-series>
```

### Label Content Options

```typescript
// Show values (default)
[dataLabel]="{ visible: true, name: 'y' }"

// Show categories
[dataLabel]="{ visible: true, name: 'x' }"

// Show custom field
[dataLabel]="{ visible: true, name: 'customField' }"

// Show percentage
[dataLabel]="{ visible: true, format: '${point.y}%' }"
```

## Label Positioning

### Position Options

Labels can be positioned at different locations relative to pie/doughnut segments:

```typescript
interface DataLabelPosition {
  position: 'Inside' | 'Outside';  // Inside or outside the segment
}
```

### Inside Position

Labels placed within the pie/doughnut segments:

```typescript
<e-accumulation-series
  [dataLabel]="{ visible: true, position: 'Inside' }">
</e-accumulation-series>
```

**Best for:**
- Pie/doughnut charts with few segments
- Preventing label overlap
- Clean, compact appearance

### Outside Position

Labels placed outside the pie/doughnut with connector lines:

```typescript
<e-accumulation-series
  [dataLabel]="{ visible: true, position: 'Outside' }">
</e-accumulation-series>
```

**Best for:**
- Many segments with complex data
- Detailed labels or percentages
- Preventing overlap

### Auto Position

Let the chart determine optimal position based on available space:

```typescript
<e-accumulation-series
  [dataLabel]="{ visible: true }">
  <!-- Position defaults to auto-adjusted -->
</e-accumulation-series>
```

### Smart Labels
Smart Labels reposition data labels automatically to avoid overlap.

**API:** `enableSmartLabels: boolean`

```html
<ejs-accumulationchart [enableSmartLabels]="true">
</ejs-accumulationchart>
```

This feature improves readability when segments are small or closely packed.

### Pyramid/Funnel Labels

For pyramid and funnel charts, labels are typically positioned inside:

```typescript
<e-accumulation-series
  type="Pyramid"
  [dataLabel]="{ 
    visible: true, 
    position: 'Inside',
    name: 'stage'
  }">
</e-accumulation-series>
```

## Label Formatting

### Format Strings

Use format placeholders to customize label display:

```typescript
// Show value with currency
[dataLabel]="{ format: '$${point.y}' }"

// Show percentage with value
[dataLabel]="{ format: '${point.x} - ${point.percentage}%' }"

// Show category and percentage
[dataLabel]="{ format: '${point.x}<br/>${point.percentage}%' }"
```

### Available Placeholders

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `${point.x}` | Category name | "Product A" |
| `${point.y}` | Y-axis value | 3500 |
| `${point.percentage}` | Percentage of total | 35 |
| `${series.name}` | Series name | "Sales" |
| `${custom_field}` | Custom data field | Any field from data object |

### Dynamic Formatting

```typescript
@Component({
  template: `
    <ejs-accumulationchart>
      <e-accumulation-series
        [dataLabel]="labelConfig">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  labelConfig = {
    visible: true,
    position: 'Inside',
    format: '${point.percentage}%'
  };
}
```

### Conditional Formatting

```typescript
onLabelRender(args: IAccumulationTextRenderEventArgs) {
  // Show percentage for values > 1000
  if (args.point.y > 1000) {
    args.text = `${args.point.percentage}%`;
  } else {
    args.text = `${args.point.x}`;
  }
}
```

## Label Templates

### Custom Label Template

Define custom HTML/template for labels using render events:

```typescript
@Component({
  template: `
    <ejs-accumulationchart (textRender)="onLabelRender($event)">
      <e-accumulation-series
        [dataLabel]="{ visible: true }">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  onLabelRender(args: IAccumulationTextRenderEventArgs) {
    // Customize label text and styling
    args.text = this.formatLabel(args);
    args.textStyle = { color: '#FFFFFF', fontWeight: 'bold' };
  }

  formatLabel(args: IAccumulationTextRenderEventArgs): string {
    const pct = args.point.percentage.toFixed(1);
    return `${args.point.x}\n${pct}%`;
  }
}
```

### Label with Icons/Images

```typescript
onLabelRender(args: IAccumulationTextRenderEventArgs) {
  const icon = args.point.y > 5000 ? '⭐' : '★';
  args.text = `${icon} ${args.point.x}`;
}
```

### Rotating Labels

```typescript
labelConfig = {
  visible: true,
  textStyle: {
    angle: -45,
    fontFamily: 'Segoe UI',
    fontStyle: 'italic'
  }
};
```

## Legends

### Basic Legend

Display a legend identifying pie/doughnut segments or data series:

```typescript
<ejs-accumulationchart>
  <e-accumulation-legend [visible]="true"></e-accumulation-legend>
  <e-accumulation-series [dataSource]="data">
  </e-accumulation-series>
</ejs-accumulationchart>
```

### Legend Properties

```typescript
interface LegendProperties {
  visible: boolean;                    // Show/hide legend
  position: 'Top' | 'Bottom' | 'Left' | 'Right'; // Legend placement
  alignment: 'Near' | 'Center' | 'Far'; // Alignment within position
  enableHighlight: boolean;            // Highlight on legend click
  toggleVisibility: boolean;           // Toggle series visibility
  legendShape: 'Circle' | 'Rectangle' | 'Triangle' | 'Diamond'; // Legend marker shape
}
```

### Legend Examples

```typescript
// Bottom legend
<e-accumulation-legend 
  [visible]="true"
  position="Bottom"
  [enableHighlight]="true">
</e-accumulation-legend>

// Right-aligned legend
<e-accumulation-legend 
  [visible]="true"
  position="Right"
  alignment="Far"
  [enableHighlight]="true">
</e-accumulation-legend>

// Custom marker shape
<e-accumulation-legend 
  [visible]="true"
  legendShape="Triangle"
  [toggleVisibility]="true">
</e-accumulation-legend>
```

## Legend Customization

### Legend Position

```typescript
@Component({
  template: `
    <ejs-accumulationchart>
      <e-accumulation-legend 
        [visible]="true"
        [position]="legendPosition"
        [alignment]="legendAlignment">
      </e-accumulation-legend>
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  legendPosition = 'Right'; // 'Top', 'Bottom', 'Left', 'Right'
  legendAlignment = 'Center'; // 'Near', 'Center', 'Far'
}
```

### Legend Item Template

Customize legend item appearance:

```typescript
onLegendRender(args: ILegendRenderEventArgs) {
  // Customize legend label and styling
  args.text = `${args.text} (Custom)`;
}
```

### Legend Width and Height

```typescript
<e-accumulation-legend 
  [visible]="true"
  width="200px"
  height="100px">
</e-accumulation-legend>
```

### Legend Label Styling

```typescript
<e-accumulation-legend 
  [visible]="true"
  [textStyle]="{ 
    color: '#666', 
    fontFamily: 'Arial', 
    fontSize: '14px' 
  }">
</e-accumulation-legend>
```

### Legend Margin and Padding

```typescript
<e-accumulation-legend 
  [visible]="true"
  margin="{ left: 10, top: 10, right: 10, bottom: 10 }">
</e-accumulation-legend>
```

## Legend Events

### Point Render Event

Customize legend appearance per data point:

```typescript
@Component({
  template: `
    <ejs-accumulationchart (legendRender)="onLegendRender($event)">
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  onLegendRender(args: ILegendRenderEventArgs) {
    // Customize legend item
    if (args.data.y > 5000) {
      args.marker.fill = '#FF6B6B';
    }
  }
}
```

### Legend Click Event

Handle legend item clicks:

```typescript
@Component({
  template: `
    <ejs-accumulationchart (legendItemClick)="onLegendClick($event)">
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  onLegendClick(args: IAccumulationLegendClickEventArgs) {
    console.log('Legend clicked:', args.data);
    // Toggle visibility or apply custom logic
  }
}
```

## Combining Labels and Legends

### Complete Example: Labels + Legend

```typescript
@Component({
  selector: 'app-pie-chart',
  template: `
    <ejs-accumulationchart 
      id="container"
      [title]="'Sales by Region'"
      (legendRender)="onLegendRender($event)">
      
      <e-accumulation-legend 
        [visible]="true"
        position="Right"
        [enableHighlight]="true">
      </e-accumulation-legend>
      
      <e-accumulation-series
        [dataSource]="salesData"
        xName="region"
        yName="sales"
        type="Pie"
        [dataLabel]="{ 
          visible: true, 
          position: 'Inside',
          format: '${point.percentage}%'
        }">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `,
  styles: [`#container { height: 420px; }`]
})
export class SalesChartComponent {
  salesData = [
    { region: 'North', sales: 12000 },
    { region: 'South', sales: 15000 },
    { region: 'East', sales: 10000 },
    { region: 'West', sales: 8000 }
  ];

  onLegendRender(args: ILegendRenderEventArgs) {
    // Customize legend marker colors
    const colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A'];
    args.marker.fill = colors[args.data.index];
  }
}
```

### Multi-Level Information Display

Combine labels and legends for comprehensive data presentation:

```typescript
<ejs-accumulationchart [title]="'Product Performance'">
  <!-- Legend on right -->
  <e-accumulation-legend 
    [visible]="true"
    position="Right">
  </e-accumulation-legend>
  
  <!-- Detailed labels inside -->
  <e-accumulation-series
    [dataSource]="data"
    type="Doughnut"
    [dataLabel]="{ 
      visible: true, 
      position: 'Inside',
      format: '${point.x}<br/>${point.y} units'
    }">
  </e-accumulation-series>
</ejs-accumulationchart>
```

## Key Takeaways

- **Labels**: Display data point information directly on chart
- **Positioning**: Use Inside/Outside based on chart complexity
- **Formatting**: Use format strings and render events for customization
- **Legends**: Identify series/categories with visual markers
- **Interaction**: Enable legend highlighting and click handling
- **Best Practice**: Use both labels and legends for comprehensive data visualization

---

## API Reference Summary

### Data Label APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `AccumulationDataLabelSettings` | Data label configuration model | [AccumulationDataLabelSettings](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationDataLabelSettings) |
| `visible` | Show/hide data labels | [visible](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationDataLabelSettings#visible) |
| `position` | Label position (Inside/Outside) | [position](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationDataLabelSettings#position) |
| `name` | Data field for label text | [name](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationDataLabelSettings#name) |
| `template` | Custom label template | [template](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationDataLabelSettings#template) |
| `connectorStyle` | Connector line styling | [connectorStyle](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationDataLabelSettings#connectorstyle) |
| `font` | Label font settings | [font](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationDataLabelSettings#font) |
| `border` | Label border styling | [border](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationDataLabelSettings#border) |

### Legend APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `LegendSettings` | Legend configuration model | [LegendSettings](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/legendSettings) |
| `visible` | Show/hide legend | [visible](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/legendSettings#visible) |
| `position` | Legend position (Top/Bottom/Left/Right) | [position](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/legendSettings#position) |
| `alignment` | Legend alignment | [alignment](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/legendSettings#alignment) |
| `toggleVisibility` | Enable legend click toggle | [toggleVisibility](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/legendSettings#togglevisibility) |
| `textStyle` | Legend text styling | [textStyle](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/legendSettings#textstyle) |

### Events

| Event | Description | Documentation Link |
|-------|-------------|-------------------|
| `textRender` | Fires before text renders | [textRender](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#textrender) |
| `legendRender` | Fires before legend renders | [legendRender](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#legendrender) |
| `legendClick` | Fires on legend click | [legendClick](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#legendclick) |

**For complete API documentation, see:** [api-reference.md](references/api-reference.md)