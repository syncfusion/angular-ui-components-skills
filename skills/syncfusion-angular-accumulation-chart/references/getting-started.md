# Getting Started with Angular Accumulation Chart

## Table of Contents
- [Installation](#installation)
- [Setup Angular Project](#setup-angular-project)
- [Basic Chart Creation](#basic-chart-creation)
- [Data Binding](#data-binding)
- [CSS Imports](#css-imports)
- [Complete Example](#complete-example)

## Installation

### Install Package via ng add

The easiest way to set up the Accumulation Chart is using the Angular CLI `ng add` command:

```bash
ng add @syncfusion/ej2-angular-charts
```

This command automatically:
- Adds the `@syncfusion/ej2-angular-charts` package and dependencies to `package.json`
- Imports required modules
- Configures theme setup

### Manual Installation

If you prefer manual installation:

```bash
npm install @syncfusion/ej2-angular-charts
```

## Setup Angular Project

### Create New Angular Project

```bash
ng new accumulation-chart-app
cd accumulation-chart-app
```

### Install Chart Package

```bash
ng add @syncfusion/ej2-angular-charts
```

## Basic Chart Creation

### Import AccumulationChartModule

In a standalone component (Angular 19+):

```typescript
import { Component } from '@angular/core';
import { AccumulationChartModule } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [AccumulationChartModule],
  template: `<ejs-accumulationchart></ejs-accumulationchart>`,
  styles: [`#container { height: 420px; width: 100%; }`]
})
export class AppComponent {}
```

In a module-based component (Angular 18 and below):

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AccumulationChartModule } from '@syncfusion/ej2-angular-charts';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, AccumulationChartModule],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

### Minimal Chart Template

```typescript
@Component({
  template: `
    <ejs-accumulationchart id="container">
        <e-accumulation-series-collection>
          <e-accumulation-series
            [dataSource]="data"
            xName="x"
            yName="y"
            type="Pie">
          </e-accumulation-series>
        </e-accumulation-series-collection>
      </ejs-accumulationchart>
  `,
  styles: [`#container { height: 420px; width: 100%; }`]
})
export class AppComponent {
  data = [
    { x: 'Item1', y: 30 },
    { x: 'Item2', y: 25 },
    { x: 'Item3', y: 20 },
    { x: 'Item4', y: 25 }
  ];
}
```

## Data Binding

### Array Data Format

Bind data as an array of objects with properties for x-axis category and y-axis value:

```typescript
export class AppComponent {
  data = [
    { x: 'Chrome', y: 37, text: '37%' },
    { x: 'Firefox', y: 28, text: '28%' },
    { x: 'Safari', y: 18, text: '18%' },
    { x: 'Others', y: 17, text: '17%' }
  ];
}
```

Specify data binding properties on the series:

```typescript
<e-accumulation-series-collection>
  <e-accumulation-series
    [dataSource]="data"
    xName="x"
    yName="y"
    type="Pie">
  </e-accumulation-series>
</e-accumulation-series-collection>
```

### Dynamic Data Binding

Update data at runtime; the chart automatically refreshes:

```typescript
addData() {
  this.data.push({ x: 'NewItem', y: 15 });
  // Chart re-renders with new data
}

replaceData() {
  this.data = [
    { x: 'A', y: 40 },
    { x: 'B', y: 60 }
  ];
}
```

### JSON Data Source

Load data from external JSON:

```typescript
import { HttpClient } from '@angular/common/http';

export class AppComponent implements OnInit {
  data: any[] = [];

  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.http.get('assets/data.json').subscribe((result: any) => {
      this.data = result;
    });
  }
}
```

## CSS Imports

### Add Theme Stylesheet

Add one of the Syncfusion themes to your `styles.css` or `styles.scss`:

```css
/* Material theme */
@import '@syncfusion/ej2-base/styles/material.css';
@import '@syncfusion/ej2-charts/styles/material.css';

/* Bootstrap theme */
@import '@syncfusion/ej2-base/styles/bootstrap.css';
@import '@syncfusion/ej2-charts/styles/bootstrap.css';

/* Fluent theme */
@import '@syncfusion/ej2-base/styles/fluent.css';
@import '@syncfusion/ej2-charts/styles/fluent.css';

/* Tailwind theme */
@import '@syncfusion/ej2-base/styles/tailwind.css';
@import '@syncfusion/ej2-charts/styles/tailwind.css';
```

### Alternative: Via Angular.json

Add to `angular.json` in the `styles` array:

```json
"styles": [
  "@syncfusion/ej2-base/styles/material.css",
  "@syncfusion/ej2-charts/styles/material.css",
  "src/styles.css"
]
```

## Complete Example

### Full Working Component

```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { AccumulationChartModule } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-pie-chart',
  standalone: true,
  imports: [CommonModule, AccumulationChartModule],
  template: `
    <div style="width: 100%; max-width: 800px; margin: 0 auto;">
      <h1>Browser Market Share</h1>
      <ejs-accumulationchart 
        id="container"
        [title]="'Browser Usage Statistics'"
        [tooltip]="{ enable: true }">
        <e-accumulation-series-collection>
          <e-accumulation-series
            [dataSource]="chartData"
            xName="browser"
            yName="percentage"
            type="Pie"
            [dataLabel]="{ 
              visible: true, 
              position: 'Inside', 
              name: 'percentage' 
            }">
          </e-accumulation-series>
        </e-accumulation-series-collection>
      </ejs-accumulationchart>
    </div>
  `,
  styles: [`#container { height: 420px; width: 100%; }`]
})
export class PieChartComponent {
  chartData = [
    { browser: 'Chrome', percentage: 37 },
    { browser: 'Firefox', percentage: 28 },
    { browser: 'Safari', percentage: 18 },
    { browser: 'Edge', percentage: 12 },
    { browser: 'Others', percentage: 5 }
  ];
}
```

### Add to App Component

```typescript
import { Component } from '@angular/core';
import { PieChartComponent } from './pie-chart/pie-chart.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [PieChartComponent],
  template: `<app-pie-chart></app-pie-chart>`
})
export class AppComponent {}
```

### Run the Application

```bash
ng serve
```

Open browser to `http://localhost:4200` and view the pie chart.

## Key Takeaways

- Use `ng add @syncfusion/ej2-angular-charts` for quick setup
- Import `AccumulationChartModule` in standalone or module components
- Bind data using `dataSource`, `xName`, and `yName` properties
- Include Syncfusion theme CSS for proper styling
- Update data at runtime for dynamic charts
- Always set chart container height and width in CSS

---

## API Reference Summary

### Core Setup APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `AccumulationChart` | Main chart component | [AccumulationChart](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart) |
| `AccumulationSeries` | Series configuration | [AccumulationSeries](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries) |
| `dataSource` | Data binding property | [dataSource](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#datasource) |
| `xName` | X-axis field name | [xName](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#xname) |
| `yName` | Y-axis field name | [yName](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#yname) |
| `type` | Chart type (Pie/Doughnut/Pyramid/Funnel) | [type](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#type) |
| `title` | Chart title text | [title](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#title) |
| `tooltip` | Tooltip configuration | [tooltip](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#tooltip) |
| `width` | Chart width | [width](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#width) |
| `height` | Chart height | [height](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#height) |

### Essential Events

| Event | Description | Documentation Link |
|-------|-------------|-------------------|
| `load` | Fires before chart loads | [load](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#load) |
| `loaded` | Fires after chart loads | [loaded](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#loaded) |
| `pointClick` | Fires on point click | [pointClick](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#pointclick) |

**For complete API documentation, see:** [api-reference.md](references/api-reference.md)