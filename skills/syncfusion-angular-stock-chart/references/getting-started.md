# Getting Started with Stock Chart

Learn how to install, set up, and create your first stock chart in an Angular application.

## Table of Contents
- [Getting Started with Stock Chart](#getting-started-with-stock-chart)
- [Installation](#installation)
  - [Step 1: Install the Syncfusion Charts Package](#step-1-install-the-syncfusion-charts-package)
  - [Alternative: Manual NPM Installation](#alternative-manual-npm-installation)
  - [Step 2: Check Angular Version Compatibility](#step-2-check-angular-version-compatibility)
- [Basic Implementation](#basic-implementation)
  - [Minimal Stock Chart](#minimal-stock-chart)
- [Data Binding](#data-binding)
  - [Data Format](#data-format)
  - [Binding Data from Array](#binding-data-from-array)
  - [Binding with DataManager](#binding-with-datamanager)
- [Verifying Your Setup](#verifying-your-setup)
  - [Check 1: Module Imports](#check-1-module-imports)
  - [Check 2: Template Structure](#check-2-template-structure)
  - [Check 3: Data Format](#check-3-data-format)
  - [Check 4: Visible Chart](#check-4-visible-chart)
- [Common Initialization Issues](#common-initialization-issues)

## Installation

### Step 1: Install the Syncfusion Charts Package

Use Angular CLI's `ng add` command to install the package and automatically configure your project:

```bash
ng add @syncfusion/ej2-angular-charts
```

This command:
- Adds `@syncfusion/ej2-angular-charts` to your `package.json`
- Installs peer dependencies automatically
- Imports necessary modules in your application
- Configures theme CSS imports

### Alternative: Manual NPM Installation

If `ng add` isn't available in your environment:

```bash
npm install @syncfusion/ej2-angular-charts
npm install @syncfusion/ej2-base @syncfusion/ej2-calendars @syncfusion/ej2-dropdowns
```

### Step 2: Check Angular Version Compatibility

Stock Chart requires:
- **Angular 19+:** Uses standalone components (default in Angular 19+)
- **Angular 12-18:** Legacy NgModule approach (see [Standalone Guide](https://ej2.syncfusion.com/angular/documentation/getting-started/angular-standalone))

Verify your version:

```bash
ng version
```

## Basic Implementation

### Minimal Stock Chart

Create a standalone component that imports the chart module:

```typescript
import { Component, ViewEncapsulation } from '@angular/core';
import { ChartAllModule, StockChartAllModule } from '@syncfusion/ej2-angular-charts';

@Component({
    imports: [ChartAllModule, StockChartAllModule],
    standalone: true,
    selector: 'app-stock',
    template: `<ejs-stockchart id='chart-container'></ejs-stockchart>`,
    encapsulation: ViewEncapsulation.None
})
export class StockChartComponent {}
```

Add to your `index.html`:

```html
<app-stock></app-stock>
```

Run your application:

```bash
ng serve
```

The chart renders with default styling and empty data.

## Data Binding

Stock Chart needs data in an array of objects with OHLC (Open, High, Low, Close) values and a date field.

### Data Format

```typescript
interface StockData {
    x: Date;           // Date for x-axis
    open: number;      // Opening price
    high: number;      // Highest price
    low: number;       // Lowest price
    close: number;     // Closing price
    volume?: number;   // Optional volume data
}
```

### Binding Data from Array

```typescript
import { Component, ViewEncapsulation } from '@angular/core';
import { ChartAllModule, StockChartAllModule } from '@syncfusion/ej2-angular-charts';

@Component({
    imports: [ChartAllModule, StockChartAllModule],
    standalone: true,
    selector: 'app-stock',
    template: `
        <ejs-stockchart id='chart-container' [dataSource]='chartData'>
            <e-stockchart-series-collection>
                <e-stockchart-series [dataSource]='chartData' type='Candle'
                    xName='x' high='high' low='low' 
                    open='open' close='close'>
                </e-stockchart-series>
            </e-stockchart-series-collection>
        </ejs-stockchart>
    `,
    encapsulation: ViewEncapsulation.None
})
export class StockChartComponent {
    chartData = [
        { x: new Date(2023, 0, 1), open: 100, high: 105, low: 95, close: 102 },
        { x: new Date(2023, 0, 2), open: 102, high: 108, low: 100, close: 105 },
        { x: new Date(2023, 0, 3), open: 105, high: 110, low: 103, close: 108 }
    ];
}
```

**Key Points:**
- `dataSource` can be on the chart or series level
- Series `xName` property maps the date field
- OHLC fields (high, low, open, close) are specified in series config
- Data should be sorted by date (ascending)

**Tips:**
- Parse date strings from API to Date objects
- Handle async loading with observables
- Consider filtering large datasets (10k+ points may impact performance)

### Binding with DataManager

For server-side data operations (filtering, sorting):

```typescript
import { Component, ViewEncapsulation } from '@angular/core';
import { DataManager, UrlAdaptor } from '@syncfusion/ej2-data';
import { ChartAllModule, StockChartAllModule } from '@syncfusion/ej2-angular-charts';

@Component({
    imports: [ChartAllModule, StockChartAllModule],
    standalone: true,
    selector: 'app-stock',
    template: `
      <ejs-stockchart
      id="stockChartSpline"
      [enablePeriodSelector]="enable"
      [chartArea]="chartArea"
      [primaryXAxis]="primaryXAxis"
      [primaryYAxis]="primaryYAxis"
      [seriesType]="seriesType"
      [indicatorType]="indicatorType">

      <e-stockchart-series-collection>
        <e-stockchart-series
          [dataSource]="dataManager"
          [query]="query"
          type="Line"
          xName="OrderDate"
          yName="Freight">
        </e-stockchart-series>
      </e-stockchart-series-collection>

    </ejs-stockchart>
    `,
    encapsulation: ViewEncapsulation.None
})
export class StockChartComponent {
     public dataManager: DataManager = new DataManager({
    url: 'https://services.syncfusion.com/angular/production/api/orders'
  });

  public query: Query = new Query().take(50);

public seriesType: string[] = ['Spline'];
  public indicatorType: string[] = [];


  public chartArea: object = {
    border: { width: 0 }
  };

  public primaryXAxis: object = {
    valueType: 'DateTime',
    crosshairTooltip: { enable: true },
    majorGridLines: { width: 0 }
  };

  public primaryYAxis: object = {
    lineStyle: { width: 0 },
    majorTickLines: { width: 0 }
  };

  public enable: boolean = true;
}
```

## Verifying Your Setup

### Check 1: Module Imports

Ensure both `ChartAllModule` and `StockChartAllModule` are in your `imports` array:

```typescript
imports: [ChartAllModule, StockChartAllModule, ...otherModules]
```

### Check 2: Template Structure

Verify your template has:
- `<ejs-stockchart>` root element
- `<e-stockchart-series-collection>` container
- At least one `<e-stockchart-series>` with type and data fields

### Check 3: Data Format

Confirm your data includes:
- `x` property with Date objects
- `high`, `low`, `open`, `close` numeric values
- Data sorted by date (ascending order recommended)

### Check 4: Visible Chart

After `ng serve`, your browser should show a chart container with candlestick series.

**If you see a blank container:**
- Check browser console for errors
- Verify CSS imports are loaded (Syncfusion theme CSS)
- Ensure data has non-zero values

**If you see "No data" message:**
- Verify data is being assigned to `chartData` property
- Check that `[dataSource]` binding is correct
- Ensure property names match (case-sensitive)

## Common Initialization Issues

**Issue: "Cannot find module '@syncfusion/ej2-angular-charts'"**
- Solution: Run `npm install @syncfusion/ej2-angular-charts`
- Or use: `ng add @syncfusion/ej2-angular-charts`

**Issue: Chart appears but no data renders**
- Solution: Verify `xName` and OHLC field names match your data
- Check that data values are not null/undefined

**Issue: Dates not formatting correctly**
- Solution: Ensure date field uses JavaScript Date objects, not strings
- Use: `new Date(dateString)` to convert strings

**Issue: Chart is too small or not visible**
- Solution: Set explicit `height` and `width` on `ejs-stockchart`
- Example: `<ejs-stockchart height='400px' width='100%'></ejs-stockchart>`
