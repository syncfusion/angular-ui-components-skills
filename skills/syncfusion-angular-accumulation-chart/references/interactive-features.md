# Interactive Features

## Table of Contents
- [Tooltips](#tooltips)
- [Selection](#selection)
- [Events](#events)
- [Point and Series Interactions](#point-and-series-interactions)
- [Highlight and Hover](#highlight-and-hover)
- [Interactive Example](#interactive-example)

## Tooltips

### Enable Tooltips

Display information when hovering over chart segments:

```typescript
<ejs-accumulationchart [tooltip]="{ enable: true }">
  <e-accumulation-series-collection>
    <e-accumulation-series [dataSource]="data">
    </e-accumulation-series>
  </e-accumulation-series-collection>
</ejs-accumulationchart>
```

### Tooltip Properties

```typescript
interface TooltipProperties {
  enable: boolean;                  // Show/hide tooltips
  visible: boolean;                 // Control visibility
  template?: string;                // Custom HTML template
  format?: string;                  // Format string with placeholders
  duration: number;                 // Display duration in ms
  opacity: number;                  // Tooltip opacity (0-1)
  border: object;                   // Border configuration
  backgroundColor: string;          // Background color
  textStyle: object;               // Font styling
  position: 'Top' | 'Bottom' | 'Right' | 'Left'; // Tooltip position
}
```

### Tooltip with Format String

```typescript
<ejs-accumulationchart 
  [tooltip]="{ 
    enable: true, 
    format: '${point.x}: ${point.y} units' 
  }">
  <e-accumulation-series-collection>
    <e-accumulation-series [dataSource]="data">
    </e-accumulation-series>
  </e-accumulation-series-collection>
</ejs-accumulationchart>
```

### Tooltip Placeholders

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `${point.x}` | Category name | "Product A" |
| `${point.y}` | Y-axis value | 3500 |
| `${point.percentage}` | Percentage | 35% |
| `${series.name}` | Series name | "Sales" |
| `${custom_field}` | Custom field | Any field from data |

### Custom Tooltip Template

```typescript
@Component({
  template: `
    <ejs-accumulationchart 
      [tooltip]="tooltipConfig"
      (tooltipRender)="onTooltipRender($event)">
      <e-accumulation-series-collection>
        <e-accumulation-series [dataSource]="data">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  tooltipConfig = {
    enable: true,
    opacity: 0.9
  };

  data = [
    { x: 'North', y: 120000, region: 'Americas' },
    { x: 'South', y: 95000, region: 'Americas' },
    { x: 'East', y: 150000, region: 'Europe' }
  ];

  onTooltipRender(args: ITooltipRenderEventArgs) {
    // Customize tooltip content and styling
    args.text = `
      <b>${args.point.x}</b><br/>
      Sales: ${args.point.y.toLocaleString()}<br/>
      Region: ${args.point.region}<br/>
      Share: ${args.point.percentage.toFixed(1)}%
    `;
    args.textStyle = { color: '#FFF', fontSize: '12px' };
  }
}
```

### Styled Tooltip

```typescript
tooltipConfig = {
  enable: true,
  border: { color: '#333', width: 1 },
  backgroundColor: '#333',
  textStyle: {
    color: '#FFFFFF',
    fontFamily: 'Segoe UI',
    fontSize: '12px'
  },
  position: 'Top',
  duration: 2000
};
```

### Shared Tooltip
Displays tooltip content for all series at once when hovering over shared points.

**API:**  
- `shared: boolean` inside `TooltipSettings`

```html
<ejs-accumulationchart
  [tooltip]="{ enable: true, shared: true }">
</ejs-accumulationchart>
```

Use shared tooltips when multiple accumulation series are rendered together.

## Selection

### Enable Selection

Allow users to select chart segments:

```typescript
<ejs-accumulationchart 
  [selectionMode]="'Point'">
  <e-accumulation-series-collection>
    <e-accumulation-series [dataSource]="data">
    </e-accumulation-series>
  </e-accumulation-series-collection>
</ejs-accumulationchart>
```

### Selection Modes

```typescript
// Select individual points
[selectionMode]="'Point'"

// Select entire series
[selectionMode]="'Series'"

// No selection
[selectionMode]="'None'"
```

### Selection Colors

Customize appearance of selected segments:

```typescript
@Component({
  template: `
    <ejs-accumulationchart 
      selectionMode="Point"
      (pointSelected)="onPointSelected($event)">
      <e-accumulation-series-collection>
        <e-accumulation-series 
          [dataSource]="data"
          (selectionComplete)="onSelectionComplete($event)">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class SelectionComponent {
  data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 },
    { x: 'C', y: 20 }
  ];

  onPointSelected(args: any) {
    console.log('Selected point:', args.pointIndex);
    // Change styling based on selection
  }

  onSelectionComplete(args: any) {
    console.log('Selection complete:', args);
  }
}
```

### Multiple Selection

```typescript
<ejs-accumulationchart 
  selectionMode="Point"
  [isMultiSelect]="true">
  <e-accumulation-series-collection>
    <e-accumulation-series [dataSource]="data">
    </e-accumulation-series>
  </e-accumulation-series-collection>
</ejs-accumulationchart>
```

With multi-select enabled, users can hold Ctrl/Cmd while clicking to select multiple segments.

## Events

### Chart Events

Respond to various chart interactions:

```typescript
@Component({
  template: `
    <ejs-accumulationchart
      (chartMouseClick)="onChartClick($event)"
      (pointRender)="onPointRender($event)"
      (seriesRender)="onSeriesRender($event)"
      (chartMouseLeave)="onChartLeave($event)">
      <e-accumulation-series-collection>
        <e-accumulation-series [dataSource]="data">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class ChartEventsComponent {
  data = [{ x: 'A', y: 30 }, { x: 'B', y: 25 }];

  onChartClick(args: IChartMouseEventArgs) {
    console.log('Chart clicked:', args);
  }

  onPointRender(args: IAccumulationEventArgs) {
    console.log('Point rendering:', args.pointIndex);
  }

  onSeriesRender(args: ISeriesRenderEventArgs) {
    console.log('Series rendering');
  }

  onChartLeave(args: IChartMouseEventArgs) {
    console.log('Mouse left chart');
  }
}
```

### Point Events

Handle point-specific interactions:

```typescript
@Component({
  template: `
    <ejs-accumulationchart
      (pointClick)="onPointClick($event)"
      (pointMove)="onPointMove($event)"
      (chartMouseMove)="onChartMouseMove($event)">
      <e-accumulation-series-collection>
        <e-accumulation-series [dataSource]="data">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class PointEventsComponent {
  data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 },
    { x: 'C', y: 20 }
  ];

  onPointClick(args: IPointEventArgs) {
    console.log('Point clicked:', {
      index: args.pointIndex,
      value: args.data.y,
      category: args.data.x
    });
  }

  onPointMove(args: IPointEventArgs) {
    // Handle point hover
    console.log('Hovering point:', args.pointIndex);
  }

  onChartMouseMove(args: IChartMouseEventArgs) {
    // Handle general mouse movement
  }
}
```

### Pointer Events
The following pointer events allow granular interaction control:

**APIs:**
- `(chartMouseDown)`  
- `(chartMouseUp)`  
- `(chartMouseMove)`  
- `(chartMouseLeave)`  

```html
<ejs-accumulationchart
  (chartMouseDown)="onMouseDown($event)"
  (chartMouseUp)="onMouseUp($event)"
  (chartMouseMove)="onMouseMove($event)"
  (chartMouseLeave)="onMouseLeave($event)">
</ejs-accumulationchart>
```


### Legend Events

```typescript
@Component({
  template: `
    <ejs-accumulationchart (legendItemClick)="onLegendClick($event)">
      <e-accumulation-legend [visible]="true">
      </e-accumulation-legend>
      <e-accumulation-series-collection>
        <e-accumulation-series [dataSource]="data">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class LegendEventsComponent {
  data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 }
  ];

  onLegendClick(args: IAccumulationLegendClickEventArgs) {
    console.log('Legend clicked:', args.data);
  }
}
```

## Point and Series Interactions

### Get Selected Points

Access selected points programmatically:

```typescript
@Component({
  template: `
    <button (click)="getSelectedPoints()">Get Selected</button>
    <ejs-accumulationchart 
      #chart
      selectionMode="Point">
      <e-accumulation-series-collection>
        <e-accumulation-series [dataSource]="data">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class SelectPointComponent {
  @ViewChild('chart') chart!: any;

  data = [
    { x: 'Q1', y: 40 },
    { x: 'Q2', y: 50 },
    { x: 'Q3', y: 60 }
  ];

  getSelectedPoints() {
    const chartInstance = this.chart.ej2_instances[0];
    const selectedIndexes = chartInstance.selectedDataIndexes;
    console.log('Selected points:', selectedIndexes);
  }
}
```

### Point-Specific Styling

```typescript
onPointRender(args: IAccumulationEventArgs) {
  // Style based on value
  if (args.point.y > 50) {
    args.fill = '#28A745'; // Green for high values
  } else if (args.point.y < 30) {
    args.fill = '#DC3545'; // Red for low values
  } else {
    args.fill = '#FFC107'; // Yellow for medium values
  }
}
```

### Series-Level Interactions

```typescript
onSeriesRender(args: ISeriesRenderEventArgs) {
  // Apply series-wide styling
  const seriesData = args.data;
  console.log('Series data:', seriesData);
}
```

## Highlight and Hover

### Enable Point Highlight

Highlight segments on hover:

```typescript
<e-accumulation-series-collection>
  <e-accumulation-series
    [dataSource]="data"
    [highlightMode]="'Point'">
  </e-accumulation-series>
</e-accumulation-series-collection>
```

### Highlight Options

```typescript
// Highlight single point on hover
[highlightMode]="'Point'"

// Highlight entire series
[highlightMode]="'Series'"

// No highlight
[highlightMode]="'None'"
```

### Custom Highlight Styling

```typescript
onPointRender(args: IAccumulationEventArgs) {
  // Store original fill for highlighting
  args.fill = args.fill || this.getColorForPoint(args.pointIndex);
}

getColorForPoint(index: number): string {
  const colors = ['#FF6B6B', '#4ECDC4', '#45B7D1'];
  return colors[index % colors.length];
}
```

### Border Highlight on Hover
Highlights segment borders dynamically based on pointer movement.

**API:**  
`enableBorderOnMouseMove: boolean`

```html
<ejs-accumulationchart
  [enableBorderOnMouseMove]="true">
</ejs-accumulationchart>
```
``

## Interactive Example

### Complete Interactive Dashboard

```typescript
@Component({
  selector: 'app-interactive-chart',
  template: `
    <div class="dashboard">
      <div class="controls">
        <h3>Interactive Accumulation Chart</h3>
        <div class="button-group">
          <button (click)="changeChartType('Pie')" [class.active]="chartType === 'Pie'">
            Pie
          </button>
          <button (click)="changeChartType('Doughnut')" [class.active]="chartType === 'Doughnut'">
            Doughnut
          </button>
          <button (click)="changeChartType('Pyramid')" [class.active]="chartType === 'Pyramid'">
            Pyramid
          </button>
          <button (click)="changeChartType('Funnel')" [class.active]="chartType === 'Funnel'">
            Funnel
          </button>
        </div>
      </div>

      <div class="chart-container">
        <ejs-accumulationchart 
          id="container"
          [tooltip]="tooltipConfig"
          selectionMode="Point"
          (pointClick)="onPointClick($event)"
          (tooltipRender)="onTooltipRender($event)">
          
          <e-accumulation-legend 
            [visible]="true"
            position="Right"
            [enableHighlight]="true">
          </e-accumulation-legend>
          
          <e-accumulation-series-collection>
            <e-accumulation-series
              [dataSource]="chartData"
              xName="category"
              yName="value"
              [type]="chartType"
              [dataLabel]="labelConfig"
              (pointRender)="onPointRender($event)">
            </e-accumulation-series>
          </e-accumulation-series-collection>
        </ejs-accumulationchart>
      </div>

      <div class="info-panel" *ngIf="selectedPoint">
        <h4>Selected Point Details</h4>
        <p>Category: {{ selectedPoint.x }}</p>
        <p>Value: {{ selectedPoint.y | number }}</p>
        <p>Percentage: {{ selectedPoint.percentage | number: '1.1-2' }}%</p>
      </div>
    </div>
  `,
  styles: [`
    .dashboard {
      padding: 20px;
      font-family: Segoe UI, Arial, sans-serif;
    }
    .controls {
      margin-bottom: 20px;
    }
    .button-group {
      display: flex;
      gap: 10px;
      margin-top: 10px;
    }
    .button-group button {
      padding: 8px 16px;
      border: 1px solid #ddd;
      background: #fff;
      cursor: pointer;
      border-radius: 4px;
      transition: all 0.3s;
    }
    .button-group button.active {
      background: #333;
      color: #fff;
      border-color: #333;
    }
    .chart-container {
      margin: 20px 0;
      height: 450px;
    }
    .info-panel {
      margin-top: 20px;
      padding: 15px;
      background: #f5f5f5;
      border-radius: 4px;
    }
    .info-panel h4 {
      margin-top: 0;
      color: #333;
    }
  `]
})
export class InteractiveChartComponent {
  chartType = 'Pie';
  selectedPoint: any = null;

  chartData = [
    { category: 'Product A', value: 35000 },
    { category: 'Product B', value: 28000 },
    { category: 'Product C', value: 34000 },
    { category: 'Product D', value: 32000 },
    { category: 'Product E', value: 40000 }
  ];

  tooltipConfig = {
    enable: true,
    format: '${point.x}: ${point.y | number}',
    opacity: 0.9
  };

  labelConfig = {
    visible: true,
    position: 'Inside',
    format: '${point.percentage}%'
  };

  changeChartType(type: string) {
    this.chartType = type;
  }

  onPointClick(args: any) {
    this.selectedPoint = args.point;
  }

  onTooltipRender(args: ITooltipRenderEventArgs) {
    const pct = args.point.percentage.toFixed(2);
    args.text = `${args.point.x}<br/>Value: ${args.point.y}<br/>Share: ${pct}%`;
  }

  onPointRender(args: IAccumulationEventArgs) {
    const colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A', '#98D8C8'];
    args.fill = colors[args.pointIndex % colors.length];
  }
}
```

## Key Takeaways

- **Tooltips**: Show detailed info on hover with custom formatting
- **Selection**: Allow users to interact with and select chart segments
- **Events**: Respond to clicks, hovers, and rendering events
- **Multi-Select**: Enable Ctrl/Cmd+click for multiple selections
- **Highlighting**: Visual feedback for hovered/selected elements
- **User Experience**: Combine events and styling for interactive dashboards

---

## API Reference Summary

### Tooltip APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `TooltipSettings` | Tooltip configuration model | [TooltipSettings](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/tooltipSettings) |
| `enable` | Enable/disable tooltip | [enable](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/tooltipSettings#enable) |
| `format` | Tooltip text format | [format](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/tooltipSettings#format) |
| `template` | Custom tooltip template | [template](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/tooltipSettings#template) |

### Selection APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `selectionMode` | Selection mode (None/Point) | [selectionMode](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#selectionmode) |
| `isMultiSelect` | Enable multiple selection | [isMultiSelect](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#ismultiselect) |
| `highlightMode` | Highlight interaction mode | [highlightMode](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#highlightmode) |

### Interactive Events

| Event | Description | Documentation Link |
|-------|-------------|-------------------|
| `pointClick` | Fires on point click | [pointClick](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#pointclick) |
| `pointMove` | Fires on point hover | [pointMove](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#pointmove) |
| `chartMouseClick` | Fires on chart click | [chartMouseClick](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#chartmouseclick) |
| `tooltipRender` | Fires before tooltip renders | [tooltipRender](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#tooltiprender) |

**For complete API documentation, see:** [api-reference.md](references/api-reference.md)