# Interactive Features

This guide explains how to add interactive behavior to the **Syncfusion Angular Accumulation Chart** (Pie / Doughnut / Funnel / Pyramid), including:

---

## Table of Contents

- [Required Imports and Providers](#required-imports-and-providers)
- [Tooltips](#tooltips)
- [Selection](#selection)
- [Highlight and Hover](#highlight-and-hover)
- [Events](#events)
- [Point and Series Interactions](#point-and-series-interactions)
- [Interactive Example](#interactive-example)
- [Key Takeaways](#key-takeaways)
- [Source Links](#source-links)

---

## Tooltips

Tooltips display additional information when hovering over chart segments.

### Required imports/providers for tooltip

```typescript
import { Component } from '@angular/core';
import {
  AccumulationChartModule,
  PieSeriesService,
  AccumulationTooltipService
} from '@syncfusion/ej2-angular-charts';
```

```typescript
@Component({
  standalone: true,
  selector: 'app-tooltip-example',
  imports: [AccumulationChartModule],
  providers: [PieSeriesService, AccumulationTooltipService],
  template: `
    <ejs-accumulationchart [tooltip]="tooltip">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class TooltipExampleComponent {
  public data = [
    { x: 'North', y: 120000 },
    { x: 'South', y: 95000 },
    { x: 'East', y: 150000 }
  ];

  public tooltip = {
    enable: true
  };
}
```

### Tooltip properties

```typescript
public tooltip = {
  enable: true,
  header: 'Sales',
  format: '${point.x}: ${point.y}',
  fill: '#333333',
  opacity: 0.9,
  border: { color: '#111111', width: 1 },
  textStyle: {
    color: '#FFFFFF',
    fontFamily: 'Segoe UI',
    size: '12px'
  },
  location: { x: 120, y: 20 },
  followPointer: true,
  shared: false
};
```

### Tooltip placeholders

You can use placeholders inside `format` such as:

- `${point.x}`
- `${point.y}`
- `${point.percentage}`
- `${series.name}`
- `${point.tooltip}` (when using `tooltipMappingName`)

### Tooltip with format string

```typescript
public tooltip = {
  enable: true,
  format: '${point.x}: ${point.y} units'
};
```

### Tooltip mapping name

```typescript
@Component({
  standalone: true,
  selector: 'app-tooltip-mapping-example',
  imports: [AccumulationChartModule],
  providers: [PieSeriesService, AccumulationTooltipService],
  template: `
    <ejs-accumulationchart [tooltip]="tooltip">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y"
          tooltipMappingName="text">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class TooltipMappingExampleComponent {
  public data = [
    { x: 'Jan', y: 13, text: 'January: 13' },
    { x: 'Feb', y: 18, text: 'February: 18' },
    { x: 'Mar', y: 22, text: 'March: 22' }
  ];

  public tooltip = {
    enable: true,
    format: '${point.tooltip}'
  };
}
```

### Custom tooltip using `tooltipRender`

```typescript
@Component({
  standalone: true,
  selector: 'app-tooltip-render-example',
  imports: [AccumulationChartModule],
  providers: [PieSeriesService, AccumulationTooltipService],
  template: `
    <ejs-accumulationchart
      [tooltip]="tooltip"
      (tooltipRender)="onTooltipRender($event)">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class TooltipRenderExampleComponent {
  public data = [
    { x: 'North', y: 120000, region: 'Americas' },
    { x: 'South', y: 95000, region: 'Americas' },
    { x: 'East', y: 150000, region: 'Europe' }
  ];

  public tooltip = {
    enable: true,
    opacity: 0.9
  };

  onTooltipRender(args: ITooltipRenderEventArgs): void {
    args.text =
      `${args.point.x}<br/>` +
      `Sales: ${args.point.y}<br/>` +
      `Share: ${args.point.percentage.toFixed(1)}%`;
  }
}
```

---

## Selection

Selection allows users to click and select chart points.

**Important:**  
For accumulation charts, valid `selectionMode` values are:
  - `'None'`
  - `'Point'`

### Required imports/providers for selection

```typescript
import { Component } from '@angular/core';
import {
  AccumulationChartModule,
  PieSeriesService,
  AccumulationSelectionService
} from '@syncfusion/ej2-angular-charts';
```

### Enable selection

```typescript
@Component({
  standalone: true,
  selector: 'app-selection-example',
  imports: [AccumulationChartModule],
  providers: [PieSeriesService, AccumulationSelectionService],
  template: `
    <ejs-accumulationchart
      selectionMode="Point"
      [selectionPattern]="'DiagonalForward'"
      (selectionComplete)="onSelectionComplete($event)">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class SelectionExampleComponent {
  public data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 },
    { x: 'C', y: 20 }
  ];

  onSelectionComplete(args: IAccSelectionCompleteEventArgs): void {
    console.log('Selection complete:', args);
  }
}
```

### Multiple selection

```typescript
<ejs-accumulationchart
  selectionMode="Point"
  [isMultiSelect]="true">
</ejs-accumulationchart>
```

### Preselect selected points

```typescript
public selectedDataIndexes = [
  { series: 0, point: 1 }
];
```

```html
<ejs-accumulationchart
  selectionMode="Point"
  [selectedDataIndexes]="selectedDataIndexes">
</ejs-accumulationchart>
```

### Get selected points programmatically

```typescript
import { Component, ViewChild } from '@angular/core';
import {
  AccumulationChartModule,
  PieSeriesService,
  AccumulationSelectionService
} from '@syncfusion/ej2-angular-charts';

@Component({
  standalone: true,
  selector: 'app-get-selected-example',
  imports: [AccumulationChartModule],
  providers: [PieSeriesService, AccumulationSelectionService],
  template: `
    <button (click)="getSelectedPoints()">Get Selected</button>

    <ejs-accumulationchart
      #chart
      selectionMode="Point">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class GetSelectedExampleComponent {
  @ViewChild('chart') chart!: any;

  public data = [
    { x: 'Q1', y: 40 },
    { x: 'Q2', y: 50 },
    { x: 'Q3', y: 60 }
  ];

  getSelectedPoints(): void {
    const chartInstance = this.chart.ej2_instances[0];
    console.log('Selected points:', chartInstance.selectedDataIndexes);
  }
}
```

---

## Highlight and Hover

Highlight shows visual feedback when hovering over a segment.

**Important:**  
For accumulation charts, valid `highlightMode` values are:
 - `'None'`
 - `'Point'`

### Required imports/providers for highlight

```typescript
import { Component } from '@angular/core';
import {
  AccumulationChartModule,
  PieSeriesService,
  AccumulationHighlightService
} from '@syncfusion/ej2-angular-charts';
```

### Enable point highlight

```typescript
@Component({
  standalone: true,
  selector: 'app-highlight-example',
  imports: [AccumulationChartModule],
  providers: [PieSeriesService, AccumulationHighlightService],
  template: `
    <ejs-accumulationchart
      highlightMode="Point"
      [enableBorderOnMouseMove]="true">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class HighlightExampleComponent {
  public data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 },
    { x: 'C', y: 20 }
  ];
}
```

### Highlight via tooltip

Tooltip settings also support `enableHighlight`:

```typescript
public tooltip = {
  enable: true,
  enableHighlight: true
};
```

### Custom point styling on render

```typescript
onPointRender(args: IAccPointRenderEventArgs): void {
  if (args.point.y > 50) {
    args.fill = '#28A745';
  } else if (args.point.y < 30) {
    args.fill = '#DC3545';
  } else {
    args.fill = '#FFC107';
  }
}
```

---

## Events

The accumulation chart supports events for clicks, hover, rendering, tooltips, legends, and selection.

### Commonly used events

- `pointClick`
- `pointMove`
- `tooltipRender`
- `selectionComplete`
- `pointRender`
- `seriesRender`
- `legendClick`
- `chartMouseClick`
- `chartMouseMove`
- `chartMouseDown`
- `chartMouseUp`
- `chartMouseLeave`

### Event example

```typescript
import { Component } from '@angular/core';
import {
  AccumulationChartModule,
  PieSeriesService,
  AccumulationTooltipService,
  AccumulationSelectionService,
  AccumulationLegendService
} from '@syncfusion/ej2-angular-charts';

@Component({
  standalone: true,
  selector: 'app-events-example',
  imports: [AccumulationChartModule],
  providers: [
    PieSeriesService,
    AccumulationTooltipService,
    AccumulationSelectionService,
    AccumulationLegendService
  ],
  template: `
    <ejs-accumulationchart
      [tooltip]="{ enable: true }"
      selectionMode="Point"
      (pointClick)="onPointClick($event)"
      (pointMove)="onPointMove($event)"
      (tooltipRender)="onTooltipRender($event)"
      (selectionComplete)="onSelectionComplete($event)"
      (legendClick)="onLegendClick($event)"
      (chartMouseClick)="onChartClick($event)"
      (chartMouseMove)="onChartMove($event)"
      (chartMouseDown)="onChartMouseDown($event)"
      (chartMouseUp)="onChartMouseUp($event)"
      (chartMouseLeave)="onChartLeave($event)">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `
})
export class EventsExampleComponent {
  public data = [
    { x: 'Q1', y: 40 },
    { x: 'Q2', y: 50 },
    { x: 'Q3', y: 60 }
  ];

  onPointClick(args: IPointEventArgs): void {
    console.log('Point clicked:', args);
  }

  onPointMove(args: IPointEventArgs): void {
    console.log('Point hovered:', args);
  }

  onTooltipRender(args: ITooltipRenderEventArgs): void {
    console.log('Tooltip render:', args);
  }

  onSelectionComplete(args: IAccSelectionCompleteEventArgs): void {
    console.log('Selection complete:', args);
  }

  onLegendClick(args: IAccLegendClickEventArgs): void {
    console.log('Legend clicked:', args);
  }

  onChartClick(args: IMouseEventArgs): void {
    console.log('Chart clicked:', args);
  }

  onChartMove(args: IMouseEventArgs): void {
    console.log('Chart moved:', args);
  }

  onChartMouseDown(args: IMouseEventArgs): void {
    console.log('Mouse down:', args);
  }

  onChartMouseUp(args: IMouseEventArgs): void {
    console.log('Mouse up:', args);
  }

  onChartLeave(args: IMouseEventArgs): void {
    console.log('Mouse left chart:', args);
  }
}
```

---

## Point and Series Interactions

### Point click details

```typescript
onPointClick(args: IPointEventArgs): void {
  console.log({
    index: args.pointIndex,
    x: args.point.x,
    y: args.point.y,
    percentage: args.point.percentage
  });
}
```

### Series render hook

```typescript
onSeriesRender(args: IAccSeriesRenderEventArgs): void {
  console.log('Series render:', args);
}
```

### Point render hook

```typescript
onPointRender(args: IAccPointRenderEventArgs): void {
  const colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A', '#98D8C8'];
  args.fill = colors[args.pointIndex % colors.length];
}
```

---

## Interactive Example

This example combines:

- Tooltip
- Selection
- Highlight
- Point click handling
- Type switching for Pie / Doughnut / Funnel / Pyramid

```typescript
import { CommonModule } from '@angular/common';
import { Component } from '@angular/core';
import {
  AccumulationChartModule,
  PieSeriesService,
  FunnelSeriesService,
  PyramidSeriesService,
  CategoryService,
  AccumulationLegendService,
  AccumulationTooltipService,
  AccumulationSelectionService,
  AccumulationHighlightService,
  AccumulationDataLabelService
} from '@syncfusion/ej2-angular-charts';

@Component({
  standalone: true,
  selector: 'app-interactive-chart',
  imports: [AccumulationChartModule, CommonModule],
  providers: [
    PieSeriesService,
    FunnelSeriesService,
    PyramidSeriesService,
    CategoryService,
    AccumulationLegendService,
    AccumulationTooltipService,
    AccumulationSelectionService,
    AccumulationHighlightService,
    AccumulationDataLabelService
  ],
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
          [tooltip]="tooltipConfig"
          selectionMode="Point"
          highlightMode="Point"
          [enableBorderOnMouseMove]="true"
          (pointClick)="onPointClick($event)"
          (tooltipRender)="onTooltipRender($event)"
          (selectionComplete)="onSelectionComplete($event)">

          <e-accumulation-series-collection>
            <e-accumulation-series
              [dataSource]="chartData"
              xName="category"
              yName="value"
              [type]="chartType === 'Doughnut' ? 'Pie' : chartType"
              [innerRadius]="chartType === 'Doughnut' ? '60%' : '0%'"
              [dataLabel]="labelConfig"
              (pointRender)="onPointRender($event)">
            </e-accumulation-series>
          </e-accumulation-series-collection>
        </ejs-accumulationchart>
      </div>

      <div class="info-panel" *ngIf="selectedPoint">
        <h4>Selected Point Details</h4>
        <p>Category: {{ selectedPoint.x }}</p>
        <p>Value: {{ selectedPoint.y }}</p>
        <p>Percentage: {{ selectedPoint.percentage }}%</p>
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
      flex-wrap: wrap;
    }

    .button-group button {
      padding: 8px 16px;
      border: 1px solid #ddd;
      background: #fff;
      cursor: pointer;
      border-radius: 4px;
      transition: all 0.3s ease;
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
  public chartType: 'Pie' | 'Doughnut' | 'Pyramid' | 'Funnel' = 'Pie';
  public selectedPoint: any = null;

  public chartData = [
    { category: 'Product A', value: 35000 },
    { category: 'Product B', value: 28000 },
    { category: 'Product C', value: 34000 },
    { category: 'Product D', value: 32000 },
    { category: 'Product E', value: 40000 }
  ];

  public tooltipConfig = {
    enable: true,
    format: '${point.x}: ${point.y}',
    opacity: 0.9
  };

  public labelConfig = {
    visible: true,
    position: 'Inside'
  };

  changeChartType(type: 'Pie' | 'Doughnut' | 'Pyramid' | 'Funnel'): void {
    this.chartType = type;
  }

  onPointClick(args: IPointEventArgs): void {
    this.selectedPoint = args.point;
  }

  onSelectionComplete(args: IAccSelectionCompleteEventArgs): void {
    console.log('Selection complete:', args);
  }

  onTooltipRender(args: ITooltipRenderEventArgs): void {
    args.text =
      `${args.point.x}<br/>` +
      `Value: ${args.point.y}<br/>` +
      `Share: ${args.point.percentage.toFixed(2)}%`;
  }

  onPointRender(args: IAccPointRenderEventArgs): void {
    const colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A', '#98D8C8'];
    args.fill = colors[args.pointIndex % colors.length];
  }
}
```

---

## Key Takeaways

- Always add the correct **providers** for the interactive feature you use.
- For **tooltip**, include `AccumulationTooltipService`.
- For **selection**, include `AccumulationSelectionService`.
- For **highlight**, include `AccumulationHighlightService`.
- For **Pie / Doughnut**, use `PieSeriesService`.
- For **Funnel**, use `FunnelSeriesService`.
- For **Pyramid**, use `PyramidSeriesService`.
- `selectionMode` for accumulation charts supports only:
  - `'None'`
  - `'Point'`
- `highlightMode` for accumulation charts supports only:
  - `'None'`
  - `'Point'`
- A doughnut chart is created by using a pie series with `innerRadius > 0`.
- Correct event names for accumulation chart include:
  - `pointClick`
  - `pointMove`
  - `tooltipRender`
  - `selectionComplete`
  - `legendClick`
  - `chartMouseClick`
  - `chartMouseMove`
  - `chartMouseDown`
  - `chartMouseUp`
  - `chartMouseLeave`

---

## Source Links

**Syncfusion Accumulation Chart Tooltip**: https://ej2.syncfusion.com/angular/documentation/accumulation-chart/tool-tip

**Syncfusion Pie / Doughnut**: https://ej2.syncfusion.com/angular/documentation/accumulation-chart/pie-dough-nut

**Syncfusion Funnel**: https://ej2.syncfusion.com/angular/documentation/accumulation-chart/funnel

**Syncfusion Pyramid**: https://ej2.syncfusion.com/angular/documentation/accumulation-chart/pyramid

**Accumulation Chart API**: https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/index-default

**Tooltip Settings API**: https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/tooltipSettings

**Selection Mode API**: https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationselectionmode

**Highlight Mode API**: https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationhighlightmode
