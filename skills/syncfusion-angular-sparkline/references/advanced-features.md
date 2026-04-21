# Advanced Features in Sparklines

## Table of Contents

- [Working with Multiple Sparklines](#working-with-multiple-sparklines)
  - [Unique ID Requirement](#unique-id-requirement)
  - [Dynamic Sparkline Generation](#dynamic-sparkline-generation)
- [Range Band](#range-band)
  - [Basic Range Band](#basic-range-band)
  - [Multiple Range Bands](#multiple-range-bands)
  - [Range Band Properties](#range-band-properties)
  - [Use Cases](#use-cases)
- [Axis Customization](#axis-customization)
  - [Setting Axis Min and Max](#setting-axis-min-and-max)
  - [Custom Interval](#custom-interval)
  - [Axis Properties Reference](#axis-properties-reference)
  - [Real Example: Percentage Data](#real-example-percentage-data)
- [User Interactions](#user-interactions)
  - [Tooltip Configuration](#tooltip-configuration)
  - [Tooltip Properties](#tooltip-properties)
- [Special Points Customization](#special-points-customization)
  - [Highlight High and Low Points](#highlight-high-and-low-points)
  - [Negative Point Color](#negative-point-color)
  - [Properties](#properties)
- [Container Sizing](#container-sizing)
  - [Fixed Size](#fixed-size)
  - [Percentage Size](#percentage-size)
  - [Dynamic Sizing](#dynamic-sizing)
- [RTL Support](#rtl-support)
  - [RTL Behavior](#rtl-behavior)
- [Real-World Examples](#real-world-examples)
  - [Server Performance Monitoring](#server-performance-monitoring)
  - [Stock Price with Zones](#stock-price-with-zones)
  - [Network Throughput Dashboard](#network-throughput-dashboard)
- [Combined Features Example](#combined-features-example)
- [Performance Optimization](#performance-optimization)

## Working with Multiple Sparklines

### Unique ID Requirement

When displaying **multiple sparkline components** on the same page, you **MUST** assign a unique `id` attribute to each sparkline. Without unique IDs, sparklines may fail to render or display incorrect data.

**Why This Matters:**
- Each sparkline creates DOM elements with ID-based references
- Duplicate or missing IDs cause rendering conflicts
- The component uses the ID to manage its internal state and SVG elements

**Example: Multiple Sparklines with Unique IDs**

```typescript
import { SparklineModule } from '@syncfusion/ej2-angular-charts'
import { Component } from '@angular/core';

@Component({
  imports: [SparklineModule],
  standalone: true,
  template: `
    <div class="dashboard">
      <!-- Sales Trend -->
      <ejs-sparkline 
        id="sales-sparkline"
        [dataSource]="salesData"
        type="Line"
        fill="#0066CC"
        height="80px">
      </ejs-sparkline>

      <!-- Revenue Trend -->
      <ejs-sparkline 
        id="revenue-sparkline"
        [dataSource]="revenueData"
        type="Column"
        fill="#4CAF50"
        height="80px">
      </ejs-sparkline>

      <!-- User Growth -->
      <ejs-sparkline 
        id="users-sparkline"
        [dataSource]="usersData"
        type="Area"
        fill="#FF9800"
        height="80px">
      </ejs-sparkline>
    </div>
  `
})
export class DashboardComponent {
  salesData = [10, 25, 15, 30, 20, 35];
  revenueData = [100, 120, 110, 150, 130, 170];
  usersData = [50, 75, 60, 90, 80, 110];
}
```

**Common Mistakes:**

```html
<!-- ✗ WRONG - No IDs -->
<ejs-sparkline [dataSource]="data1"></ejs-sparkline>
<ejs-sparkline [dataSource]="data2"></ejs-sparkline>

<!-- ✗ WRONG - Duplicate IDs -->
<ejs-sparkline id="sparkline" [dataSource]="data1"></ejs-sparkline>
<ejs-sparkline id="sparkline" [dataSource]="data2"></ejs-sparkline>

<!-- ✓ CORRECT - Unique IDs -->
<ejs-sparkline id="sparkline-1" [dataSource]="data1"></ejs-sparkline>
<ejs-sparkline id="sparkline-2" [dataSource]="data2"></ejs-sparkline>
```

### Dynamic Sparkline Generation

When generating sparklines dynamically with `*ngFor`, use unique identifiers from your data:

```typescript
@Component({
  template: `
    <div class="metrics-grid">
      <div *ngFor="let metric of metrics" class="metric-card">
        <h4>{{ metric.name }}</h4>
        <ejs-sparkline 
          [id]="'sparkline-' + metric.id"
          [dataSource]="metric.values"
          [type]="metric.type"
          [fill]="metric.color"
          height="60px">
        </ejs-sparkline>
      </div>
    </div>
  `
})
export class MetricsDashboard {
  metrics = [
    { 
      id: 'sales', 
      name: 'Sales', 
      values: [10, 25, 15, 30], 
      type: 'Line', 
      color: '#0066CC' 
    },
    { 
      id: 'revenue', 
      name: 'Revenue', 
      values: [100, 120, 110, 150], 
      type: 'Column', 
      color: '#4CAF50' 
    },
    { 
      id: 'users', 
      name: 'Users', 
      values: [50, 75, 60, 90], 
      type: 'Area', 
      color: '#FF9800' 
    }
  ];
}
```

**ID Naming Best Practices:**
- Use descriptive names: `"sales-trend"`, `"revenue-chart"`, `"user-growth"`
- Use consistent prefixes: `"sparkline-{purpose}"`, `"chart-{metric}"`
- For dynamic generation: `"sparkline-" + uniqueIdentifier`
- Avoid special characters; stick to alphanumeric and hyphens

## Range Band

Range bands display shaded background regions to highlight data ranges or thresholds.

### Basic Range Band

Add a shaded region showing a normal operating range:

```typescript
import { SparklineModule } from '@syncfusion/ej2-angular-charts'
import { Component } from '@angular/core';

@Component({
  imports: [SparklineModule],
  standalone: true,
  template: `<ejs-sparkline 
    [dataSource]="data"
    type="Area"
    [rangeBandSettings]="{ 
      start: 20,
      end: 30,
      color: '#E3F2FD',
      opacity: 0.4
    }">
  </ejs-sparkline>`
})
export class RangeBandComponent {
  data = [10, 25, 15, 35, 28, 22, 40]
}
```

**Output:** A light blue shaded region from Y=20 to Y=30, showing the acceptable range.

### Multiple Range Bands

Highlight multiple ranges (e.g., good, warning, danger):

```typescript
@Component({
  template: `<ejs-sparkline id='container' width='150px' height='150px' lineWidth= 2 fill = '#0d3c9b' [rangeBandSettings] = 'rangeBandSettings' [dataSource]="data">
    </ejs-sparkline>`
})
export class MultiRangeBandComponent {
   public data: object[] = [ 0, 6, 4, 1, 3, 2, 5 ] as any;
    public rangeBandSettings: object[] = [{
        startRange: 1,
        endRange: 3,
        color: '#bfd4fc',
        opacity:0.4
        }];
}
```

### Range Band Properties

| Property | Type | Purpose |
|----------|------|---------|
| `start` | number | Starting Y value for the range |
| `end` | number | Ending Y value for the range |
| `color` | string | Background color (hex or named) |
| `opacity` | number | Transparency (0-1) |

### Use Cases

- **SLA Monitoring** - Show acceptable vs. breach ranges
- **Performance Metrics** - Display normal operating zones
- **Temperature Control** - Highlight ideal temperature bands
- **Quality Assurance** - Mark acceptable product ranges

## Axis Customization

Customize the axis bounds and interval for better data representation.

### Setting Axis Min and Max

Control the data range displayed:

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="data"
    type="Column"
    [axisSettings]="{ 
      minX: 0,
      maxX: 10,
      minY: 0,
      maxY: 100,
      valueType: 'Numeric'
    }">
  </ejs-sparkline>`
})
export class AxisRangeComponent {
  data = [20, 50, 30, 80, 60]
}
```

**Effect:** The Y-axis scales from 0 to 100 (not auto-fitted to data).

### Custom Interval

Set the Y-axis interval for specific tick spacing:

```typescript
axisSettings = {
  minY: 0,
  maxY: 100,
  intervalY: 25  // Intervals at 0, 25, 50, 75, 100
}
```

### Axis Properties Reference

| Property | Type | Purpose |
|----------|------|---------|
| `minX` | number | Minimum X-axis value |
| `maxX` | number | Maximum X-axis value |
| `minY` | number | Minimum Y-axis value |
| `maxY` | number | Maximum Y-axis value |
| `intervalX` | number | X-axis interval spacing |
| `intervalY` | number | Y-axis interval spacing |
| `valueType` | string | Data type: 'Numeric', 'Category', 'DateTime' |

### Real Example: Percentage Data

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="conversionRates"
    xName="week"
    yName="percentage"
    type="Line"
    [axisSettings]="{ 
      minY: 0,
      maxY: 100
    }">
  </ejs-sparkline>`
})
export class ConversionComponent {
  conversionRates = [
    { week: 'W1', percentage: 45 },
    { week: 'W2', percentage: 58 },
    { week: 'W3', percentage: 62 }
  ]
}
```

## User Interactions

### Tooltip Configuration

Customize tooltip appearance and behavior:

```typescript
import { SparklineModule, SparklineTooltipService } from '@syncfusion/ej2-angular-charts'

@Component({
  imports: [SparklineModule],
  standalone: true,
  providers: [SparklineTooltipService],
  template: `<ejs-sparkline 
    [dataSource]="data"
    [tooltipSettings]="{ 
      visible: true,
      fill: '#1A1A1A',
      border: { color: '#FFA500' }
    }">
  </ejs-sparkline>`
})
export class TooltipConfigComponent {
  data = [10, 25, 15, 30, 20]
}
```

### Tooltip Properties

| Property | Type | Purpose |
|----------|------|---------|
| `visible` | boolean | Enable/disable tooltip |
| `format` | string | Format string with ${x}, ${y} |
| `fill` | string | Background color |
| `border.color` | string | Border color |
| `opacity` | number | Transparency |
| `textStyle.color` | string | Text color |

## Special Points Customization

Customize colors for high, low, and negative points:

### Highlight High and Low Points

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="data"
    type="Column"
    [highPointColor]="'#00B050'"
    [lowPointColor]="'#FF0000'">
  </ejs-sparkline>`
})
export class SpecialPointsComponent {
  data = [20, 50, 30, 80, 60, 15, 75]
}
```

**Output:** Highest column (80) is green, lowest (15) is red.

### Negative Point Color

Color negative values differently:

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="data"
    type="Line"
    [negativePointColor]="'#FF5733'">
  </ejs-sparkline>`
})
export class NegativePointComponent {
  data = [10, -5, 15, -3, 8, 12]
}
```

### Properties

| Property | Type | Purpose |
|----------|------|---------|
| `highPointColor` | string | Color for highest point |
| `lowPointColor` | string | Color for lowest point |
| `negativePointColor` | string | Color for negative values |
| `startPointColor` | string | Color for first point |
| `endPointColor` | string | Color for last point |

## Container Sizing

### Fixed Size

Set explicit dimensions:

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="data"
    height="100px"
    width="300px">
  </ejs-sparkline>`
})
export class FixedSizeComponent {
  data = [2, 6, 4, 8, 5]
}
```

### Percentage Size

Use percentage for responsive sizing:

```typescript
@Component({
  template: `<div style="width: 500px;">
    <ejs-sparkline 
      [dataSource]="data"
      height="100px"
      width="100%">
    </ejs-sparkline>
  </div>`
})
export class ResponsiveSizeComponent {
  data = [2, 6, 4, 8, 5]
}
```

### Dynamic Sizing

Adjust size based on data:

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="data"
    [height]="getHeight()"
    [width]="getWidth()">
  </ejs-sparkline>`
})
export class DynamicSizeComponent {
  data = [2, 6, 4, 8, 5]
  
  getHeight(): string {
    return this.data.length > 10 ? '150px' : '100px'
  }
  
  getWidth(): string {
    return this.data.length * 20 + 'px'
  }
}
```

## RTL Support

Enable Right-to-Left layout for RTL languages (Arabic, Hebrew, etc.):

```typescript
@Component({
  template: `<div dir="rtl">
    <ejs-sparkline 
      [dataSource]="data"
      type="Column"
      [enableRtl]="true">
    </ejs-sparkline>
  </div>`
})
export class RTLComponent {
  data = [10, 25, 15, 30, 20]
}
```

### RTL Behavior

- X-axis labels appear right-aligned
- Animation flows from right to left
- Tooltips position appropriately for RTL space

## Real-World Examples

### Server Performance Monitoring

```typescript
@Component({
  imports: [SparklineModule],
  standalone: true,
  template: `<div>
    <h4>CPU Usage Last 24h</h4>
    <ejs-sparkline id='container' width='150px' height='150px' lineWidth= 2 fill = '#0d3c9b' [rangeBandSettings] = 'rangeBandSettings' [dataSource]="cpuUsage" xName='hour' yName='percent'>
    </ejs-sparkline>
  </div>`
})
export class ServerMonitorComponent {
 cpuUsage = [
        { hour: 0, percent: 35 },
        { hour: 1, percent: 42 },
        { hour: 2, percent: 88 },  // Peak
        { hour: 3, percent: 45 }
      ]
      public rangeBandSettings: object[] = [{
        startRange: 35,
        endRange: 67,
        color: 'green',
        opacity:0.4
        }];
}
```

### Stock Price with Zones

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="stockPrice"
    xName="day"
    yName="price"
    type="Line"
    valueType='Category'
    [dataLabelSettings]="{ visible: ['Start', 'End'] }"
    [rangeBandSettings]="[
      { startRange: 100, endRange: 110, color: '#4CAF50', opacity: 0.1 },  
      { startRange: 130, endRange: 140, color: '#F44336', opacity: 0.1 } 
    ]">
  </ejs-sparkline>`
})
export class StockPriceComponent {
  stockPrice = [
    { day: 'Mon', price: 105 },
    { day: 'Tue', price: 115 },
    { day: 'Wed', price: 135 },
    { day: 'Thu', price: 128 }
  ]
}
```

### Network Throughput Dashboard

```typescript
@Component({
  template: `<div>
    <ejs-sparkline 
      [dataSource]="throughput"
      type="Column"
      height="120px"
      [axisSettings]="{ maxY: 1000 }"
      [highPointColor]="'#4CAF50'"
      [tooltipSettings]="{ 
        visible: true
      }">
    </ejs-sparkline>
  </div>`
})
export class NetworkComponent {
  throughput = [
    150, 280, 320, 450, 390, 520, 480, 600
  ]
}
```

## Combined Features Example

Complete example with multiple advanced features:

```typescript
@Component({
  imports: [SparklineModule],
  standalone: true,
  providers: [SparklineTooltipService],
  template: `<ejs-sparkline 
    [dataSource]="monthlyMetrics"
    xName="month"
    yName="value"
    type="Area"
    valueType="Category"
    height="150px"
    width="100%"
    [axisSettings]="{ minY: 0, maxY: 100 }"
    [rangeBandSettings]="{ start: 70, end: 100, color: '#90EE90', opacity: 0.3 }"
    [markerSettings]="{ visible: ['High', 'Low'], size: 8 }"
    [dataLabelSettings]="{ visible: ['High', 'Low'] }"
    [tooltipSettings]="{ visible: true, format: '${x}: ${y}%' }"
    [highPointColor]="'#4CAF50'"
    [lowPointColor]="'#FF5252'">
  </ejs-sparkline>`
})
export class CompleteExampleComponent {
  monthlyMetrics = [
    { month: 'Jan', value: 65 },
    { month: 'Feb', value: 75 },
    { month: 'Mar', value: 80 },
    { month: 'Apr', value: 85 },
    { month: 'May', value: 82 },
    { month: 'Jun', value: 90 }
  ]
}
```

**Features Combined:**
- ✓ Area visualization
- ✓ Range band highlighting success zone
- ✓ Markers on extremes
- ✓ Data labels
- ✓ Custom tooltips
- ✓ Special point colors
- ✓ Responsive width

## Performance Optimization

For sparklines with advanced features and large datasets:

1. **Avoid 'All' markers/labels** - Use specific types
2. **Limit range bands** - 2-3 bands maximum
3. **Use Column type** - Renders faster with 1000+ points
4. **Simplify tooltips** - Short format strings
5. **Debounce updates** - When binding to streams

```typescript
// ✓ Good for performance
[markerSettings]="{ visible: ['High', 'Low'] }"
[dataLabelSettings]="{ visible: ['End'] }"

// ✗ Poor for 1000+ points
[markerSettings]="{ visible: ['All'] }"
[dataLabelSettings]="{ visible: ['All'] }"
```


## Responsive Design

### Mobile-Optimized Sparklines

Create responsive sparklines that adapt to different screen sizes:

```typescript
import { SparklineModule } from '@syncfusion/ej2-angular-charts'
import { CommonModule } from '@angular/common'
import { Component } from '@angular/core'

@Component({
  imports: [SparklineModule, CommonModule],
  standalone: true,
  template: `<div class="sparkline-container">
    <div class="sparkline-item">
      <h5>Revenue</h5>
      <ejs-sparkline 
        [dataSource]="revenueData"
        xName="month"
        yName="revenue"
        type="Area"
        [height]="getMobileHeight()"
        [width]="getMobileWidth()">
      </ejs-sparkline>
    </div>
  </div>`,
  styles: [`
    .sparkline-container {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
      padding: 20px;
    }
    .sparkline-item {
      padding: 15px;
      border: 1px solid #e0e0e0;
      border-radius: 4px;
    }
    @media (max-width: 768px) {
      .sparkline-container {
        grid-template-columns: 1fr;
      }
    }
  `]
})
export class ResponsiveSparklineComponent {
  revenueData = [
    { month: 'Jan', revenue: 45000 },
    { month: 'Feb', revenue: 52000 },
    { month: 'Mar', revenue: 48000 },
    { month: 'Apr', revenue: 65000 },
    { month: 'May', revenue: 72000 },
    { month: 'Jun', revenue: 68000 }
  ]

  getMobileHeight(): string {
    if (window.innerWidth < 768) {
      return '80px'
    }
    return '120px'
  }

  getMobileWidth(): string {
    if (window.innerWidth < 768) {
      return '100%'
    }
    return '100%'
  }
}
```

## Accessibility and Screen Reader Support

### WCAG 2.1 Compliance

Ensure sparklines are accessible to users with disabilities:

```typescript
import { SparklineModule, SparklineTooltipService } from '@syncfusion/ej2-angular-charts'
import { CommonModule } from '@angular/common'
import { Component } from '@angular/core'

@Component({
  imports: [SparklineModule, CommonModule],
  standalone: true,
  providers: [SparklineTooltipService],
  template: `<div role="region" [attr.aria-label]="'Sales trend chart showing quarterly data'">
    <h3>Quarterly Sales Trend</h3>
    <ejs-sparkline 
      [dataSource]="salesData"
      xName="quarter"
      yName="sales"
      type="Line"
      [tooltipSettings]="{ 
        visible: true,
        format: '\${quarter}: \${sales} units'
      }"
      role="img"
      [attr.aria-label]="'Line chart displaying quarterly sales from Q1 to Q4'">
    </ejs-sparkline>
    <div role="region" [attr.aria-label]="'Sales data table'" class="data-table">
      <table>
        <caption>Quarterly Sales Data</caption>
        <thead>
          <tr>
            <th>Quarter</th>
            <th>Sales (Units)</th>
          </tr>
        </thead>
        <tbody>
          <tr *ngFor="let item of salesData">
            <td>{{ item.quarter }}</td>
            <td>{{ item.sales }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>`,
  styles: [`
    .data-table {
      margin-top: 20px;
      border: 1px solid #ccc;
      padding: 10px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid #ddd;
      padding: 8px;
      text-align: left;
    }
    th {
      background-color: #f5f5f5;
      font-weight: bold;
    }
  `]
})
export class AccessibleSparklineComponent {
  salesData = [
    { quarter: 'Q1', sales: 25000 },
    { quarter: 'Q2', sales: 35000 },
    { quarter: 'Q3', sales: 32000 },
    { quarter: 'Q4', sales: 48000 }
  ]
}
```

## Event Handling

Sparklines support various events to handle user interactions and lifecycle changes.

### Available Events

| Event | Trigger | Use Case |
|-------|---------|----------|
| `loaded` | After sparkline finishes rendering | Initialize custom logic, logging |
| `sparklineMouseClick` | User clicks anywhere on sparkline | Navigate to details page |
| `sparklineMouseMove` | Mouse moves over sparkline | Custom tooltip behavior |
| `pointRegionMouseClick` | User clicks specific data point | Show detailed metrics |
| `pointRegionMouseMove` | Mouse moves over data point | Highlight related data |
| `resize` | Sparkline container resizes | Recalculate dimensions |

### Point Click Event

Handle clicks on individual data points:

```typescript
import { SparklineModule } from '@syncfusion/ej2-angular-charts'
import { Component } from '@angular/core'

@Component({
  imports: [SparklineModule],
  standalone: true,
  template: `<div>
    <ejs-sparkline 
      [dataSource]="salesData"
      xName="month"
      yName="sales"
      type="Column"
      (pointRegionMouseClick)="onPointClick($event)">
    </ejs-sparkline>
    <div *ngIf="selectedPoint">
      <h4>Details for {{ selectedPoint.x }}</h4>
      <p>Sales: {{ selectedPoint.y }}</p>
    </div>
  </div>`
})
export class PointClickComponent {
  salesData = [
    { month: 'Jan', sales: 10000 },
    { month: 'Feb', sales: 15000 },
    { month: 'Mar', sales: 12000 },
    { month: 'Apr', sales: 18000 }
  ]
  selectedPoint: any = null

  onPointClick(args: any) {
    this.selectedPoint = {
      x: args.pointIndex,
      y: args.value
    }
    console.log('Point clicked:', args.pointIndex, args.value)
  }
}
```

### Sparkline Click Event

Handle clicks on the entire sparkline:

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="data"
    type="Line"
    (sparklineMouseClick)="onSparklineClick($event)">
  </ejs-sparkline>`
})
export class SparklineClickComponent {
  data = [10, 25, 15, 35, 20, 30]

  onSparklineClick(args: any) {
    console.log('Sparkline clicked')
    // Navigate to detailed chart view
    // this.router.navigate(['/details'])
  }
}
```

### Loaded Event

Execute logic after sparkline finishes rendering:

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="data"
    type="Area"
    (loaded)="onLoaded($event)">
  </ejs-sparkline>`
})
export class LoadedEventComponent {
  data = [10, 25, 15, 35, 20]

  onLoaded(args: any) {
    console.log('Sparkline loaded successfully')
    // Add custom SVG elements or annotations
    // Track analytics
  }
}
```

### Mouse Move Event

Track mouse movement for custom interactions:

```typescript
@Component({
  template: `<div>
    <ejs-sparkline 
      [dataSource]="data"
      type="Line"
      (pointRegionMouseMove)="onMouseMove($event)">
    </ejs-sparkline>
    <div class="hover-info">
      Hovering over point: {{ hoveredIndex }}
    </div>
  </div>`
})
export class MouseMoveComponent {
  data = [10, 25, 15, 35, 20, 30]
  hoveredIndex: number = -1

  onMouseMove(args: any) {
    this.hoveredIndex = args.pointIndex
  }
}
```

### Resize Event

Handle sparkline container resize:

```typescript
@Component({
  template: `<ejs-sparkline 
    [dataSource]="data"
    type="Column"
    [height]="height"
    [width]="width"
    (resize)="onResize($event)">
  </ejs-sparkline>`
})
export class ResizeComponent {
  data = [10, 25, 15, 35, 20]
  height = '100px'
  width = '100%'

  onResize(args: any) {
    console.log('Sparkline resized:', args.currentSize)
    // Adjust marker sizes or label fonts based on new dimensions
  }
}
```

### Combined Event Handling

Use multiple events together:

```typescript
@Component({
  imports: [SparklineModule, CommonModule],
  standalone: true,
  template: `<div class="event-demo">
    <ejs-sparkline 
      [dataSource]="performanceData"
      xName="hour"
      yName="cpu"
      type="Line"
      [markerSettings]="{ visible: ['High', 'Low'] }"
      (loaded)="onLoaded($event)"
      (pointRegionMouseClick)="onPointClick($event)"
      (sparklineMouseClick)="onSparklineClick($event)">
    </ejs-sparkline>
    <div class="event-log">
      <h4>Event Log:</h4>
      <ul>
        <li *ngFor="let event of eventLog">{{ event }}</li>
      </ul>
    </div>
  </div>`
})
export class CombinedEventsComponent {
  performanceData = [
    { hour: 0, cpu: 35 },
    { hour: 1, cpu: 42 },
    { hour: 2, cpu: 88 },
    { hour: 3, cpu: 45 }
  ]
  eventLog: string[] = []

  onLoaded(args: any) {
    this.logEvent('Sparkline loaded')
  }

  onPointClick(args: any) {
    this.logEvent(`Point ${args.pointIndex} clicked: ${args.value}%`)
  }

  onSparklineClick(args: any) {
    this.logEvent('Sparkline container clicked')
  }

  logEvent(message: string) {
    this.eventLog.unshift(`[${new Date().toLocaleTimeString()}] ${message}`)
    if (this.eventLog.length > 10) {
      this.eventLog.pop()
    }
  }
}
```

## Print and Export

Export sparklines to image formats for reports and documentation.

### Export to Image

Export sparkline as PNG, JPEG, or SVG:

```typescript
import { SparklineModule } from '@syncfusion/ej2-angular-charts'
import { Component, ViewChild } from '@angular/core'
import { SparklineComponent } from '@syncfusion/ej2-angular-charts'

@Component({
  imports: [SparklineModule],
  standalone: true,
  template: `<div>
    <ejs-sparkline 
      #sparkline
      [dataSource]="data"
      type="Line">
    </ejs-sparkline>
    <button (click)="exportPNG()">Export as PNG</button>
    <button (click)="exportJPEG()">Export as JPEG</button>
    <button (click)="exportSVG()">Export as SVG</button>
  </div>`
})
export class ExportComponent {
  @ViewChild('sparkline') sparkline!: SparklineComponent
  data = [10, 25, 15, 35, 20, 30]

  exportPNG() {
    this.sparkline.export('PNG', 'Sparkline')
  }

  exportJPEG() {
    this.sparkline.export('JPEG', 'Sparkline')
  }

  exportSVG() {
    this.sparkline.export('SVG', 'Sparkline')
  }
}
```

### Print Sparkline

Print the sparkline directly:

```typescript
@Component({
  template: `<div>
    <ejs-sparkline 
      #sparkline
      [dataSource]="data"
      type="Area">
    </ejs-sparkline>
    <button (click)="printSparkline()">Print</button>
  </div>`
})
export class PrintComponent {
  @ViewChild('sparkline') sparkline!: SparklineComponent
  data = [10, 25, 15, 35, 20, 30]

  printSparkline() {
    this.sparkline.print()
  }
}
```

## Best Practices Summary

| Practice | Benefit | Example |
|----------|---------|---------|
| Use appropriate type | Better data representation | Pie for composition, WinLoss for binary |
| Limit markers/labels | Improved readability | High/Low only, not All |
| Apply range bands | Context and thresholds | Green/yellow/red zones |
| Enable tooltips | User understanding | Hover for exact values |
| Responsive sizing | Mobile compatibility | 60px mobile, 100px desktop |
| Semantic colors | Accessibility | High contrast ratios (4.5:1+) |
| Lazy loading | Performance at scale | Page 6 datasets per page |
| Include data table | Screen reader support | WCAG 2.1 AA compliant |

