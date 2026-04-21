# Series Types & Data Binding

The Range Navigator supports multiple series types for different data visualization needs. This guide covers available series types, data binding methods, and configuration options.

## Table of Contents

- [Available Series Types](#available-series-types)
- [Area Series](#area-series)
  - [Basic Area Series](#basic-area-series)
  - [Area Series with Styling](#area-series-with-styling)
- [Line Series](#line-series)
  - [Basic Line Series](#basic-line-series)
  - [Line Series with Custom Styling](#line-series-with-custom-styling)
- [StepLine Series](#stepline-series)
  - [Basic StepLine Series](#basic-stepline-series)
- [Multiple Series](#multiple-series)
  - [Two Series Configuration](#two-series-configuration)
- [Data Binding](#data-binding)
  - [Local Array Binding](#local-array-binding)
  - [Remote Data Binding](#remote-data-binding)
  - [Dynamic Data Update](#dynamic-data-update)
- [Series Configuration](#series-configuration)
  - [xName and yName Mapping](#xname-and-yname-mapping)
  - [Custom Series Naming](#custom-series-naming)
- [Series Styling](#series-styling)
  - [Color and Fill](#color-and-fill)
  - [Gradient Fill](#gradient-fill)
- [Best Practices](#best-practices)
- [Service Provider Reference](#service-provider-reference)

## Available Series Types

| Type | Purpose | Best For |
|------|---------|----------|
| `Area` | Filled area visualization | Continuous data trends, cumulative values |
| `Line` | Line chart rendering | Trend visualization, time-series data |
| `StepLine` | Step-wise line | Step-function data, state changes |

## Area Series

Area series is the default and most commonly used series type for Range Navigator.

### Basic Area Series

```typescript
@Component({
  template: `
    <ejs-rangenavigator 
      id="rn-container"

      valueType="DateTime">
      <e-rangenavigator-series-collection>
        <e-rangenavigator-series 
          [dataSource]="areaData"
          xName="date" 
          yName="value" 
          type="Area">
        </e-rangenavigator-series>
      </e-rangenavigator-series-collection>
    </ejs-rangenavigator>
  `
})
export class AreaSeriesComponent {
  areaData = [
    { date: new Date(2023, 0, 1), value: 50 },
    { date: new Date(2023, 0, 2), value: 55 },
    { date: new Date(2023, 0, 3), value: 60 },
    { date: new Date(2023, 0, 4), value: 58 }
  ];
}
```

### Area Series with Styling

```typescript
@Component({
  template: `
    <ejs-rangenavigator>
      <e-rangenavigator-series-collection>
        <e-rangenavigator-series 
          xName="date" 
          yName="value" 
          type="Area"
          [fill]="'#3498db'"
          [border]="{ color: '#2c3e50', width: 2 }">
        </e-rangenavigator-series>
      </e-rangenavigator-series-collection>
    </ejs-rangenavigator>
  `
})
export class StyledAreaComponent { }
```

## Line Series

Line series displays data as a simple line without fill.

### Basic Line Series

```typescript
@Component({
  template: `
    <ejs-rangenavigator 
      valueType="DateTime">
      <e-rangenavigator-series-collection>
        <e-rangenavigator-series 
          [dataSource]="lineData"
          xName="date" 
          yName="value" 
          type="Line">
        </e-rangenavigator-series>
      </e-rangenavigator-series-collection>
    </ejs-rangenavigator>
  `
})
export class LineSeriesComponent {
  lineData = [
    { date: new Date(2023, 0, 1), value: 100 },
    { date: new Date(2023, 0, 2), value: 110 },
    { date: new Date(2023, 0, 3), value: 105 },
    { date: new Date(2023, 0, 4), value: 115 }
  ];
}
```

### Line Series with Custom Styling

```typescript
import { Component } from '@angular/core';
import { 
    RangeNavigatorModule, 
    LineSeriesService, 
    DateTimeService 
} from '@syncfusion/ej2-angular-charts';

@Component({
    selector: 'app-container',
    standalone: true,
    imports: [RangeNavigatorModule],
    providers: [LineSeriesService, DateTimeService],
    template: `
        <ejs-rangenavigator id="rangeNavigator" valueType="DateTime">
            <e-rangenavigator-series-collection>
                <e-rangenavigator-series 
                    [dataSource]="data"
                    xName="x" 
                    yName="y" 
                    type="Line"
                    [width]="3"
                    fill="#e74c3c">
                </e-rangenavigator-series>
            </e-rangenavigator-series-collection>
        </ejs-rangenavigator>`
})
export class AppComponent {
    public data: Object[] = [
        { x: new Date(2023, 0, 1), y: 10 },
        { x: new Date(2023, 1, 1), y: 25 },
        { x: new Date(2023, 2, 1), y: 15 }
    ];
}

```

## StepLine Series

Step Line series displays data with step-wise lines connecting points.

### Basic StepLine Series

```typescript
import { NgModule } from '@angular/core'
import { BrowserModule } from '@angular/platform-browser'
import { ChartModule, RangeNavigatorModule } from '@syncfusion/ej2-angular-charts'
import { StepLineSeriesService} from '@syncfusion/ej2-angular-charts'




import { Component, OnInit } from '@angular/core';
import { datasrc } from './datasource';

@Component({
imports: [
         ChartModule, RangeNavigatorModule
    ],

providers: [ StepLineSeriesService ],
standalone: true,
    selector: 'app-container',
    template: `<ejs-rangenavigator id="rn-container" [value]='value'>
            <e-rangenavigator-series-collection>
                <e-rangenavigator-series [dataSource]='chartData' type='StepLine' xName='x' yName='y' width=2>
                </e-rangenavigator-series>
            </e-rangenavigator-series-collection>
        </ejs-rangenavigator>`
})
export class AppComponent implements OnInit {
    public value?: Object[];
    public chartData?: Object[];
    public tooltip?: Object[];
    public labelFormat?: string;
    ngOnInit(): void {
        this.value = [12,30];
        this.chartData = datasrc;
        this.labelFormat = 'MMM-yy';
    }
}
```

## Multiple Series

Range Navigator supports multiple series for comparative analysis.

### Two Series Configuration

```typescript
@Component({
  template: `
    <ejs-rangenavigator 
      valueType="DateTime">
      <e-rangenavigator-series-collection>
        <e-rangenavigator-series 
          [dataSource]="multiSeriesData"
          xName="date" 
          yName="series1" 
          type="Area"
          [fill]="'#3498db'">
        </e-rangenavigator-series>
        <e-rangenavigator-series 
          [dataSource]="multiSeriesData"
          xName="date" 
          yName="series2" 
          type="Area"
          [fill]="'#e74c3c'">
        </e-rangenavigator-series>
      </e-rangenavigator-series-collection>
    </ejs-rangenavigator>
  `
})
export class MultiSeriesComponent {
  multiSeriesData = [
    { date: new Date(2023, 0, 1), series1: 50, series2: 40 },
    { date: new Date(2023, 0, 2), series1: 60, series2: 45 },
    { date: new Date(2023, 0, 3), series1: 55, series2: 50 }
  ];
}
```

## Data Binding

### Local Array Binding

```typescript
export class LocalDataComponent {
  @Component({
    template: `
      <ejs-rangenavigator [dataSource]="localData">
        <!-- series -->
      </ejs-rangenavigator>
    `
  })
  class MyComponent {
    localData = [
      { x: new Date(2023, 0, 1), y: 21 },
      { x: new Date(2023, 0, 2), y: 24 },
      { x: new Date(2023, 0, 3), y: 36 }
    ];
  }
}
```

### Remote Data Binding

```typescript
import { HttpClient } from '@angular/common/http';

@Component({
  imports: [RangeNavigatorComponent, CommonModule],
  template: `
    <ejs-rangenavigator >
      <e-rangenavigator-series-collection>
        <e-rangenavigator-series 
          xName="date" 
          yName="close" 
          type="Area"
          [dataSource]="remoteData">
        </e-rangenavigator-series>
      </e-rangenavigator-series-collection>
    </ejs-rangenavigator>
  `
})
export class RemoteDataComponent implements OnInit {
  remoteData: any;

  constructor(private http: HttpClient) { }

  ngOnInit(): void {
    this.http.get('https://api.example.com/stock-data')
      .subscribe(data => this.remoteData = data);
  }
}
```

### Dynamic Data Update

```typescript
@Component({
  template: `
    <button (click)="updateData()">Update Data</button>
    <ejs-rangenavigator [dataSource]="chartData">
      <!-- series -->
    </ejs-rangenavigator>
  `
})
export class DynamicDataComponent {
  chartData = [
    { date: new Date(2023, 0, 1), value: 50 }
  ];

  updateData(): void {
    this.chartData = [
      { date: new Date(2023, 0, 1), value: 50 },
      { date: new Date(2023, 0, 2), value: 65 },
      { date: new Date(2023, 0, 3), value: 75 }
    ];
  }
}
```

## Series Configuration

### xName and yName Mapping

Always map your data object properties to xName and yName:

```typescript
// Data structure
import { Component } from '@angular/core';
import { 
  RangeNavigatorModule, 
  AreaSeriesService, 
  DateTimeService 
} from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-container',
  standalone: true,
  imports: [RangeNavigatorModule],
  // DateTimeService is required because 'timestamp' is a Date object
  providers: [AreaSeriesService, DateTimeService],
  template: `
    <ejs-rangenavigator 
      id="rangeNavigator" 
      valueType="DateTime">
      <e-rangenavigator-series-collection>
        <e-rangenavigator-series 
          [dataSource]="data"
          xName="timestamp"    
          yName="measurement"  
          type="Area">
        </e-rangenavigator-series>
      </e-rangenavigator-series-collection>
    </ejs-rangenavigator>
  `
})
export class AppComponent {
  public data: Object[] = [
    { timestamp: new Date(2023, 0, 1), measurement: 100 },
    { timestamp: new Date(2023, 0, 2), measurement: 120 }
  ];
}

```

### Custom Series Naming

```typescript
@Component({
  template: `
    <e-rangenavigator-series 
      xName="date" 
      yName="value" 
      type="Area"
      name="Revenue">
    </e-rangenavigator-series>
  `
})
export class NamedSeriesComponent { }
```

## Series Styling

### Color and Fill

```typescript
@Component({
  template: `
    <e-rangenavigator-series 
      xName="x" 
      yName="y" 
      type="Area"
      [fill]="'#2ecc71'"
      [opacity]="0.7"
      [border]="{ color: '#27ae60', width: 2 }">
    </e-rangenavigator-series>
  `
})
export class ColoredSeriesComponent { }
```

### Gradient Fill

```typescript
@Component({
  template: `
    <e-rangenavigator-series 
      xName="x" 
      yName="y" 
      type="Area"
      [fill]="gradientFill">
    </e-rangenavigator-series>
  `
})
export class GradientSeriesComponent {
  gradientFill = 'url(#gradient)';
}
```

## Best Practices

1. **Choose Appropriate Series Type:**
   - Area: Continuous data with cumulative values
   - Line: Trend visualization
   - StepLine: Discrete state changes

2. **Data Property Mapping:** Always use correct property names in xName and yName

3. **Multiple Series:** Use for comparative visualization but limit to 2-3 series for clarity

4. **Empty Points:** Handle null/undefined values appropriately

5. **Performance:** For large datasets (>10k points), consider data aggregation or sampling

6. **Service Injection:** Always provide required services (AreaSeriesService, LineSeriesService, etc.)

## Service Provider Reference

Each series type requires its corresponding service to be injected:

| Series Type | Required Service |
|-------------|------------------|
| Area | `AreaSeriesService` |
| Line | `LineSeriesService` |
| StepLine | `StepLineSeriesService` |

```typescript
import {
  AreaSeriesService,
  LineSeriesService,
  StepLineSeriesService,
  DateTimeService,
  RangeTooltipService
} from '@syncfusion/ej2-angular-charts';

@Component({
  providers: [
    AreaSeriesService,        // For Area series
    LineSeriesService,        // For Line series
    StepLineSeriesService,    // For StepLine series
    DateTimeService,          // For DateTime axis
    RangeTooltipService       // For tooltips
  ]
})
export class MyComponent { }
```
