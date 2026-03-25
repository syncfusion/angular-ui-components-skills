# Data Binding Reference for Syncfusion Angular Chart

## Table of Contents

- [Introduction](#introduction)
- [Data Binding Approaches Overview](#data-binding-approaches-overview)
- [Local Data Binding](#local-data-binding)
  - [Simple Local Data](#simple-local-data)
  - [Common DataSource](#common-datasource)
  - [Complex JSON Structures](#complex-json-structures)
- [Remote Data Binding](#remote-data-binding)
  - [Using DataManager](#using-datamanager)
  - [OData Services](#odata-services)
  - [ODataV4 Services](#odatav4-services)
  - [Web API Adaptor](#web-api-adaptor)
  - [Custom Adaptor](#custom-adaptor)
- [Lazy Loading](#lazy-loading)
- [Dynamic Data Updates](#dynamic-data-updates)
  - [Adding Data Points](#adding-data-points)
  - [Removing Data Points](#removing-data-points)
  - [Replacing Entire Dataset](#replacing-entire-dataset)
  - [Interactive Add/Remove](#interactive-addremove)
- [Handling Empty Points](#handling-empty-points)
- [Offline Mode](#offline-mode)
- [No Data Template](#no-data-template)
- [Performance Optimization](#performance-optimization)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Advanced Scenarios](#advanced-scenarios)

## Introduction

The Syncfusion Angular Chart component provides flexible data binding capabilities to accommodate various application scenarios. Whether you're working with local JSON arrays, fetching data from remote services, or handling real-time data streams, the chart component offers multiple approaches to suit your needs.

This reference guide covers all data binding methods, performance considerations, and best practices for working with data in Angular Chart components.

## Data Binding Approaches Overview

The chart supports the following data binding methods:

| Method | Best For | Advantages | Considerations |
|--------|----------|------------|-----------------|
| Local data | Small to medium datasets | Simple setup, no network latency, instant rendering | All data must be in memory |
| Common datasource | Multiple series sharing data | Reduces redundancy, single update point | Limited to data common across series |
| Lazy loading | Large datasets with scrolling | Loads only visible data, better performance | Requires server-side pagination |
| Remote data (OData/WebAPI) | Backend-driven data | Scalable, centralized data management, real-time updates | Network latency, requires service setup |
| Offline mode | Data caching with client-side actions | Eliminates repeated requests, faster interactions | Initial load time, memory constraints |

## Local Data Binding

### Simple Local Data

Bind JSON data directly to chart series using the `dataSource` property. Map JSON fields to x and y axes using `xName` and `yName` properties.

**Basic Example:**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-chart',
  template: `
    <ejs-chart>
      <e-series-collection>
        <e-series 
          [dataSource]='chartData' 
          xName='month' 
          yName='sales'
          type='Column'>
        </e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class ChartComponent {
  public chartData: Object[] = [
    { month: 'Jan', sales: 35 },
    { month: 'Feb', sales: 28 },
    { month: 'Mar', sales: 34 },
    { month: 'Apr', sales: 32 },
    { month: 'May', sales: 40 },
    { month: 'Jun', sales: 32 }
  ];
}
```

### Common DataSource

Share a single data source across multiple series by setting `dataSource` at the chart level instead of the series level.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-chart',
  template: `
    <ejs-chart [dataSource]='commonData'>
      <e-series-collection>
        <e-series xName='x' yName='y1' type='Line' name='Product A'></e-series>
        <e-series xName='x' yName='y2' type='Line' name='Product B'></e-series>
        <e-series xName='x' yName='y3' type='Line' name='Product C'></e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class ChartComponent {
  public commonData: Object[] = [
    { x: 'Jan', y1: 35, y2: 28, y3: 34 },
    { x: 'Feb', y1: 28, y2: 35, y3: 32 },
    { x: 'Mar', y1: 34, y2: 32, y3: 40 },
    { x: 'Apr', y1: 32, y2: 40, y3: 32 }
  ];
}
```

**Benefits:**
- Single source of truth for data updates
- Reduced code duplication
- Easier maintenance for multi-series charts

### Complex JSON Structures

Handle nested JSON data by mapping to nested properties:

```typescript
public complexData: Object[] = [
  { 
    date: '2024-01', 
    metrics: { 
      revenue: 50000, 
      expenses: 35000,
      profit: 15000 
    },
    metadata: {
      region: 'North',
      category: 'Electronics'
    }
  },
  // ... more data
];

// Map nested properties
<e-series 
  [dataSource]='complexData' 
  xName='date' 
  yName='metrics.revenue'
  type='Column'>
</e-series>
```

## Remote Data Binding

### Using DataManager

The `DataManager` class simplifies communication with REST APIs, OData services, and custom web endpoints.

**Basic Remote Data Example:**

```typescript
import { Component } from '@angular/core';
import { DataManager, Query, ODataAdaptor } from '@syncfusion/ej2-data';

@Component({
  selector: 'app-chart',
  template: `
    <ejs-chart>
      <e-series-collection>
        <e-series 
          [dataSource]='remoteData' 
          [query]='query'
          xName='OrderDate' 
          yName='Freight'
          type='Line'>
        </e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class ChartComponent {
  public remoteData: DataManager = new DataManager({
    url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Orders/',
    adaptor: new ODataAdaptor()
  });
  
  public query: Query = new Query().take(10).where('EmployeeID', 'equal', 3);
}
```

### OData Services

OData (Open Data Protocol) is a standardized protocol for creating and consuming data APIs.

```typescript
import { ODataAdaptor } from '@syncfusion/ej2-data';

public odataManager: DataManager = new DataManager({
  url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Orders/',
  adaptor: new ODataAdaptor(),
  crossDomain: true
});

public query: Query = new Query()
  .select(['OrderID', 'CustomerID', 'Freight', 'OrderDate'])
  .take(100)
  .sortBy('OrderDate', 'descending');
```

**Query Operations:**
- `.select()` - Specify fields to retrieve
- `.where()` - Filter data
- `.sortBy()` - Sort results
- `.take()` - Limit number of records
- `.skip()` - Skip records (pagination)

### ODataV4 Services

ODataV4 is the latest version with enhanced query capabilities:

```typescript
import { ODataV4Adaptor } from '@syncfusion/ej2-data';

public odataV4Manager: DataManager = new DataManager({
  url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Orders/',
  adaptor: new ODataV4Adaptor()
});

public advancedQuery: Query = new Query()
  .select(['OrderID', 'ShipCountry', 'Freight'])
  .where('Freight', 'greaterthan', 50)
  .take(20)
  .expand('Customer');  // ODataV4 specific
```

### Web API Adaptor

For custom REST APIs that don't follow OData conventions:

```typescript
import { WebApiAdaptor } from '@syncfusion/ej2-data';

public webApiManager: DataManager = new DataManager({
  url: 'https://your-api.com/api/sales',
  adaptor: new WebApiAdaptor()
});
```

**Expected Response Format:**

```json
{
  "Items": [
    { "id": 1, "month": "Jan", "sales": 50000 },
    { "id": 2, "month": "Feb", "sales": 55000 }
  ],
  "Count": 830
}
```

### Custom Adaptor

Create custom adaptors for specialized data transformations:

```typescript
import { ODataAdaptor } from '@syncfusion/ej2-data';

export class CustomAdaptor extends ODataAdaptor {
  processResponse(): Object {
    let result: any = super.processResponse.apply(this, arguments);
    
    // Add serial numbers
    result.forEach((item: any, index: number) => {
      item.serialNo = index + 1;
    });
    
    // Transform dates
    result.forEach((item: any) => {
      if (item.date) {
        item.formattedDate = new Date(item.date).toLocaleDateString();
      }
    });
    
    // Calculate computed fields
    result.forEach((item: any) => {
      if (item.revenue && item.cost) {
        item.profit = item.revenue - item.cost;
        item.margin = ((item.profit / item.revenue) * 100).toFixed(2);
      }
    });
    
    return result;
  }
}

// Usage
public customData: DataManager = new DataManager({
  url: 'https://your-api.com/data',
  adaptor: new CustomAdaptor()
});
```

## Lazy Loading

Lazy loading enables on-demand data retrieval for large datasets, loading only visible data ranges.

```typescript
import { Component, ViewChild } from '@angular/core';
import { ChartComponent } from '@syncfusion/ej2-angular-charts';
import { IScrollEventArgs } from '@syncfusion/ej2-charts';

@Component({
  selector: 'app-chart',
  template: `
    <ejs-chart #chart
      [primaryXAxis]='primaryXAxis'
      [zoomSettings]='zoomSettings'
      (scrollEnd)='scrollEnd($event)'>
      <e-series-collection>
        <e-series 
          [dataSource]='chartData' 
          xName='x' 
          yName='y'
          type='Line'>
        </e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class LazyLoadChartComponent {
  @ViewChild('chart') chart: ChartComponent;
  
  public chartData: Object[] = this.getInitialData();
  
  public primaryXAxis: Object = {
    valueType: 'DateTime',
    scrollbarSettings: {
      enable: true
    }
  };
  
  public zoomSettings: Object = {
    enableScrollbar: true,
    enableSelectionZooming: true
  };
  
  getInitialData(): Object[] {
    let data: Object[] = [];
    let startDate = new Date(2024, 0, 1);
    for (let i = 0; i < 100; i++) {
      data.push({
        x: new Date(startDate.getTime() + i * 24 * 60 * 60 * 1000),
        y: Math.random() * 100
      });
    }
    return data;
  }
  
  scrollEnd(args: IScrollEventArgs): void {
    if (args.currentRange) {
      let min = args.currentRange.minimum;
      let max = args.currentRange.maximum;
      
      // Fetch data for the visible range
      this.fetchDataForRange(min, max).then(newData => {
        // Append new data
        this.chartData = [...this.chartData, ...newData];
        this.chart.refresh();
      });
    }
  }
  
  async fetchDataForRange(min: number, max: number): Promise<Object[]> {
    // Simulate API call
    return new Promise((resolve) => {
      setTimeout(() => {
        let data: Object[] = [];
        let minDate = new Date(min);
        let maxDate = new Date(max);
        // Generate data for range
        resolve(data);
      }, 500);
    });
  }
}
```

## Dynamic Data Updates

### Adding Data Points

Use the `addPoint` method to append new data points:

```typescript
import { Component, ViewChild } from '@angular/core';
import { ChartComponent } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-chart',
  template: `
    <button (click)="addNewPoint()">Add Point</button>
    <ejs-chart #chart>
      <e-series-collection>
        <e-series [dataSource]='chartData' xName='x' yName='y'></e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class DynamicChartComponent {
  @ViewChild('chart') chart: ChartComponent;
  
  public chartData: Object[] = [
    { x: 'Jan', y: 35 },
    { x: 'Feb', y: 28 }
  ];
  
  addNewPoint(): void {
    let newPoint = { x: 'Mar', y: Math.floor(Math.random() * 50) };
    this.chart.series[0].addPoint(newPoint, 500); // 500ms animation
  }
}
```

### Removing Data Points

Use `removePoint` to delete data points by index:

```typescript
removePointAtIndex(index: number): void {
  if (index >= 0 && index < this.chartData.length) {
    this.chart.series[0].removePoint(index, 500);
  }
}
```

### Replacing Entire Dataset

Use `setData` for complete data refresh:

```typescript
refreshData(): void {
  let newData = this.fetchNewDataSet();
  this.chart.series[0].setData(newData, 1000); // 1000ms animation
}
```

### Interactive Add/Remove

Enable users to add/remove points by clicking:

```typescript
@Component({
  template: `
    <ejs-chart (chartMouseClick)='chartMouseClick($event)'>
      <!-- chart configuration -->
    </ejs-chart>
  `
})
export class InteractiveChartComponent {
  chartMouseClick(args: IMouseEventArgs): void {
    if (args.target.includes('Point')) {
      // Remove clicked point
      let pointIndex = args.pointIndex;
      this.chart.series[0].removePoint(pointIndex);
    } else {
      // Add point at clicked location
      let xValue = args.axisData['primaryXAxis'];
      let yValue = args.axisData['primaryYAxis'];
      let newPoint = { x: xValue, y: yValue };
      this.chart.series[0].addPoint(newPoint);
    }
  }
}
```

## Handling Empty Points

Data points with `null` or `undefined` values are treated as empty points.

```typescript
public dataWithEmptyPoints: Object[] = [
  { x: 'Jan', y: 35 },
  { x: 'Feb', y: null },    // Empty point
  { x: 'Mar', y: 34 },
  { x: 'Apr', y: undefined }, // Empty point
  { x: 'May', y: 40 }
];

// Configure empty point behavior
<e-series 
  [dataSource]='dataWithEmptyPoints'
  [emptyPointSettings]='emptyPointSettings'>
</e-series>

public emptyPointSettings: Object = {
  mode: 'Average',  // 'Gap', 'Zero', 'Average', 'Drop'
  fill: '#ff6347',  // Custom color for empty points
  border: {
    width: 2,
    color: '#000000'
  }
};
```

**Empty Point Modes:**
- `Gap` - Leave blank space (default)
- `Zero` - Treat as zero value
- `Average` - Calculate average of adjacent points
- `Drop` - Remove from series entirely

## Offline Mode

Enable offline mode to load all data once and handle operations client-side:

```typescript
import { DataManager, ODataAdaptor } from '@syncfusion/ej2-data';

public offlineData: DataManager = new DataManager({
  url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Orders/',
  adaptor: new ODataAdaptor(),
  offline: true  // Enable offline mode
});
```

**Use Cases:**
- Small to medium datasets
- Reducing server load
- Improving responsiveness
- Working with intermittent connectivity

**Considerations:**
- Entire dataset loaded at initialization
- Higher initial load time
- Increased memory usage

## No Data Template

Display custom content when no data is available:

```typescript
@Component({
  template: `
    <ejs-chart [noDataTemplate]='noDataTemplate'>
      <e-series-collection>
        <e-series [dataSource]='emptyData'></e-series>
      </e-series-collection>
    </ejs-chart>
    
    <ng-template #noDataTemplate>
      <div style="text-align: center; padding: 50px;">
        <h3>No Data Available</h3>
        <p>Please load data to view the chart</p>
        <button (click)="loadData()">Load Data</button>
      </div>
    </ng-template>
  `
})
export class NoDataChartComponent {
  public emptyData: Object[] = [];
  
  loadData(): void {
    this.emptyData = this.fetchData();
  }
}
```

## Performance Optimization

### Large Datasets

For datasets with thousands of points:

1. **Use Canvas Rendering:**
```typescript
<ejs-chart [enableCanvas]='true'>
```

2. **Disable Animations:**
```typescript
public animation: Object = { enable: false };
```

3. **Implement Data Virtualization:**
```typescript
// Load data in chunks
public chartData: Object[] = [];

ngOnInit() {
  this.loadDataChunk(0, 1000); // Load first 1000 points
}

loadDataChunk(start: number, count: number) {
  let chunk = this.fetchDataRange(start, count);
  this.chartData = [...this.chartData, ...chunk];
}
```

4. **Use Aggregation:**
```typescript
// Aggregate data before binding
public aggregatedData = this.aggregateByMonth(rawData);
```

### Throttling Updates

For real-time data streams:

```typescript
import { Subject } from 'rxjs';
import { throttleTime } from 'rxjs/operators';

export class RealTimeChartComponent {
  private dataStream = new Subject<any>();
  
  ngOnInit() {
    this.dataStream.pipe(
      throttleTime(1000) // Update chart max once per second
    ).subscribe(data => {
      this.updateChart(data);
    });
  }
  
  onDataReceived(newData: any) {
    this.dataStream.next(newData);
  }
}
```

## Best Practices

1. **Choose the Right Data Binding Method:**
   - Local data for small, static datasets
   - Remote data for large, server-managed data
   - Lazy loading for very large scrollable datasets

2. **Optimize Data Structure:**
   - Keep JSON flat when possible
   - Use consistent field naming
   - Remove unnecessary fields

3. **Handle Loading States:**
```typescript
public isLoading: boolean = true;

fetchData() {
  this.isLoading = true;
  this.dataService.getData().subscribe(
    data => {
      this.chartData = data;
      this.isLoading = false;
    },
    error => {
      console.error('Error loading data:', error);
      this.isLoading = false;
    }
  );
}
```

4. **Implement Error Handling:**
```typescript
public errorMessage: string = '';

loadRemoteData() {
  this.dataManager.executeQuery(this.query)
    .then((e: any) => {
      this.chartData = e.result;
    })
    .catch(error => {
      this.errorMessage = 'Failed to load data: ' + error.message;
      console.error(error);
    });
}
```

5. **Memory Management:**
```typescript
ngOnDestroy() {
  // Clear large datasets
  this.chartData = [];
  // Unsubscribe from observables
  this.subscription?.unsubscribe();
}
```

## Troubleshooting

### Data Not Displaying

**Issue:** Chart shows no data
**Solutions:**
- Verify `xName` and `yName` match data field names (case-sensitive)
- Check browser console for errors
- Ensure data is not null or undefined
- Verify data format matches series type

```typescript
// Debug data binding
console.log('Chart Data:', this.chartData);
console.log('X Field:', this.xName);
console.log('Y Field:', this.yName);
```

### CORS Errors with Remote Data

**Issue:** Cross-origin request blocked
**Solutions:**
- Enable CORS on server
- Use proxy configuration in development
- Set `crossDomain: true` in DataManager

```typescript
public remoteData: DataManager = new DataManager({
  url: 'https://api.example.com/data',
  adaptor: new WebApiAdaptor(),
  crossDomain: true,
  headers: [{ 'Content-Type': 'application/json' }]
});
```

### Performance Issues

**Issue:** Chart slow with large datasets
**Solutions:**
- Enable canvas rendering
- Implement data aggregation
- Use lazy loading
- Reduce point count through sampling

### Empty Points Not Working

**Issue:** Empty points not rendered as expected
**Solutions:**
- Ensure values are exactly `null` or `undefined`
- Check `emptyPointSettings.mode` configuration
- Verify series type supports empty points

## Advanced Scenarios

### Real-Time Data Streaming

```typescript
import { WebSocketSubject } from 'rxjs/webSocket';

export class RealTimeChartComponent {
  private socket$: WebSocketSubject<any>;
  public chartData: Object[] = [];
  private maxPoints: number = 50;
  
  ngOnInit() {
    this.socket$ = new WebSocketSubject('ws://localhost:8080/data');
    
    this.socket$.subscribe(
      data => {
        this.chartData.push(data);
        
        // Keep only last N points
        if (this.chartData.length > this.maxPoints) {
          this.chartData.shift();
        }
        
        this.chart.series[0].setData(this.chartData);
      }
    );
  }
  
  ngOnDestroy() {
    this.socket$.complete();
  }
}
```

### Combining Multiple Data Sources

```typescript
import { forkJoin } from 'rxjs';

loadMultipleDataSources() {
  forkJoin({
    sales: this.api.getSalesData(),
    forecast: this.api.getForecastData(),
    targets: this.api.getTargetsData()
  }).subscribe(results => {
    this.chartData = this.combineData(results);
  });
}

combineData(results: any): Object[] {
  return results.sales.map((item: any, index: number) => ({
    month: item.month,
    sales: item.value,
    forecast: results.forecast[index]?.value,
    target: results.targets[index]?.value
  }));
}
```

### Caching Strategy

```typescript
import { Injectable } from '@angular/core';
import { Observable, of } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class ChartDataService {
  private cache = new Map<string, any>();
  private cacheDuration = 5 * 60 * 1000; // 5 minutes
  
  getData(key: string): Observable<any> {
    let cached = this.cache.get(key);
    
    if (cached && Date.now() - cached.timestamp < this.cacheDuration) {
      return of(cached.data);
    }
    
    return this.http.get(`/api/data/${key}`).pipe(
      tap(data => {
        this.cache.set(key, {
          data: data,
          timestamp: Date.now()
        });
      })
    );
  }
}
```

## API Reference Summary

### Data Binding Properties

| Property | Type | Description | API Reference |
|----------|------|-------------|---------------|
| dataSource | Object[] \| DataManager | Chart data source | [ChartModel.dataSource](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#dataSource), [Series.dataSource](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#dataSource) |
| xName | string | X-axis field name | [Series.xName](https://ej2.syncfusion.com/angular/documentation/api/chart/series#xName) |
| yName | string | Y-axis field name | [Series.yName](https://ej2.syncfusion.com/angular/documentation/api/chart/series#yName) |
| query | Query | DataManager query | [Series.query](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#query) |
| high | string | High value field (financial/range series) | [Series.high](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#high) |
| low | string | Low value field (financial/range series) | [Series.low](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#low) |
| open | string | Open value field (financial series) | [Series.open](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#open) |
| close | string | Close value field (financial series) | [Series.close](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#close) |
| size | string | Size field (bubble series) | [Series.size](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#size) |
| pointColorMapping | string | Color field for point coloring | [Series.pointColorMapping](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#pointColorMapping) |
| colorName | string | Color mapping field for range colors | [Series.colorName](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#colorName) |

### Empty Point Handling

| API | Description | Documentation |
|-----|-------------|---------------|
| EmptyPointSettingsModel | Empty point configuration interface | [emptyPointSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/emptyPointSettingsModel) |
| EmptyPointSettings | Empty point settings class | [emptyPointSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/emptyPointSettings) |
| EmptyPointMode | Empty point handling modes (Gap, Zero, Average, Drop) | [emptyPointMode.md](https://ej2.syncfusion.com/angular/documentation/api/chart/emptyPointMode) |

**Series Property:** [Series.emptyPointSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#emptyPointSettings)

### Financial Data Fields

| API | Description | Documentation |
|-----|-------------|---------------|
| FinancialDataFields | Interface for OHLC data structure | [financialDataFields.md](https://ej2.syncfusion.com/angular/documentation/api/chart/financialDataFields) |

### Data Events

| Event | Interface | Description | API Reference |
|-------|-----------|-------------|---------------|
| load | ILoadedEventArgs | Before chart loads - ideal for data preparation | [ChartModel.load](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#load) |
| loaded | ILoadedEventArgs | After chart loads with data | [ChartModel.loaded](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#loaded) |

### Performance Properties

| Property | Type | Default | Description | API Reference |
|----------|------|---------|-------------|---------------|
| enableCanvas | boolean | false | Use canvas rendering for large datasets | [ChartModel.enableCanvas](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#enableCanvas) |
| enableAnimation | boolean | true | Enable/disable animations | [ChartModel.enableAnimation](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#enableAnimation) |
| enableComplexProperty | boolean | false | Improve performance through data mapping | [Series.enableComplexProperty](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#enableComplexProperty) |

### Data Manager Integration

**External Library:** @syncfusion/ej2-data

**Key Classes:**
- DataManager - Main data management class
- Query - Query builder for filtering, sorting, paging
- Predicate - Condition builder
- ODataAdaptor - OData service adapter
- WebApiAdaptor - ASP.NET Web API adapter
- UrlAdaptor - Generic REST API adapter
- CustomDataAdaptor - Custom adapter implementation

**Documentation:** See [ej2-data documentation](https://ej2.syncfusion.com/angular/documentation/data/)

---
