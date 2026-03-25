# Series Types and Configuration

## Table of Contents
- [Series Type Overview](#series-type-overview)
- [Pie Chart](#pie-chart)
- [Doughnut Chart](#doughnut-chart)
- [Pyramid Chart](#pyramid-chart)
- [Funnel Chart](#funnel-chart)
- [Comparing Chart Types](#comparing-chart-types)
- [Multiple Series](#multiple-series)
- [Series Properties](#series-properties)

## Series Type Overview

Accumulation charts support four primary series types, each suited for different data visualization scenarios:

| Type | Best For | Key Feature |
|------|----------|-------------|
| Pie | Part-to-whole relationships | Simple, circular segments |
| Doughnut | Part-to-whole with center content (implemented via Pie + innerRadius) | Hollow center for labels/metrics |
| Pyramid | Hierarchical/funnel data | Diminishing stack top-down |
| Funnel | Step-wise conversion data | Funnel-shaped segments |

## Pie Chart

### Basic Pie Chart

The pie chart displays data as circular slices, where each slice's size represents its proportional value.

```typescript
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
```

### Pie Chart with Start Angle

Control pie rotation using `startAngle` (0-360 degrees):

```typescript
<e-accumulation-series-collection>
  <e-accumulation-series
    [dataSource]="data"
    xName="x"
    yName="y"
    type="Pie"
    [startAngle]="270">
  </e-accumulation-series>
</e-accumulation-series-collection>
```

### Pie Chart with Explode

Separate a slice from the pie center:

```typescript
<e-accumulation-series-collection>
  <e-accumulation-series
    [dataSource]="data"
    xName="x"
    yName="y"
    type="Pie"
    [explode]="true"
    [explodeIndex]="2">
  </e-accumulation-series>
</e-accumulation-series-collection>
```

Explode all slices on hover:

```typescript
<ejs-accumulationchart 
  [pointRender]="onPointRender">
  <e-accumulation-series-collection>
    <!-- series defined here -->
  </e-accumulation-series-collection>
</ejs-accumulationchart>

onPointRender(args: IAccumulationTextRenderEventArgs) {
  // Dynamically set explode based on interaction
}
```

### Additional Explosion Properties

#### explodeAll
Explodes all points in the pie/doughnut chart simultaneously.

**API:** `explodeAll: boolean`

```html
<e-accumulation-series
  type="Pie"
  [explode]="true"
  [explodeAll]="true">
</e-accumulation-series>
```

#### explodeOffset
Defines the outward offset distance for exploded slices.

**API:** `explodeOffset: string`

```html
<e-accumulation-series
  type="Pie"
  explodeOffset="15%">
</e-accumulation-series>
```

### Pie Chart Complete Example

```typescript
@Component({
  template: `
    <ejs-accumulationchart id="container" [title]="'Sales Distribution'">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="salesData"
          xName="product"
          yName="sales"
          type="Pie"
          [dataLabel]="{ 
            visible: true, 
            position: 'Inside', 
            name: 'text' 
          }">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `,
  styles: [`#container { height: 420px; }`]
})
export class PieChartComponent {
  salesData = [
    { product: 'Product A', sales: 35, text: 'A-35%' },
    { product: 'Product B', sales: 28, text: 'B-28%' },
    { product: 'Product C', sales: 18, text: 'C-18%' },
    { product: 'Product D', sales: 19, text: 'D-19%' }
  ];
}
```

## Doughnut (Pie with innerRadius)

### Basic Doughnut

There is no separate "Doughnut" chart type. To create a doughnut, use `type="Pie"` and set an `innerRadius` (for example `'40%'`) to create the hollow center.

```typescript
<e-accumulation-series-collection>
  <e-accumulation-series
    [dataSource]="data"
    xName="x"
    yName="y"
    type="Pie"
    [innerRadius]="'40%'">
  </e-accumulation-series>
</e-accumulation-series-collection>
```

### Doughnut with Center Label (Pie + innerRadius)

Use the hollow center to display a key metric or label. Implement this with `type="Pie"` and an `innerRadius`:

```typescript
@Component({
  template: `
    <ejs-accumulationchart id="container">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y"
          type="Pie"
          [innerRadius]="'40%'"
          [dataLabel]="{ visible: true, position: 'Inside' }">
        </e-accumulation-series>
      </e-accumulation-series-collection>
      <!-- Center content rendered separately -->
      <div class="center-label">
        <div class="metric">$45,000</div>
        <div class="label">Total Revenue</div>
      </div>
    </ejs-accumulationchart>
  `,
  styles: [`
    #container { height: 420px; position: relative; }
    .center-label { 
      position: absolute; 
      top: 50%; 
      left: 50%; 
      transform: translate(-50%, -50%);
      text-align: center;
    }
    .metric { font-size: 24px; font-weight: bold; }
    .label { font-size: 14px; color: #666; }
  `]
})
export class DoughnutComponent {
  data = [
    { x: 'North', y: 12000 },
    { x: 'South', y: 15000 },
    { x: 'East', y: 10000 },
    { x: 'West', y: 8000 }
  ];
}
```

### Hollow Doughnut with Multiple Rings

Create nested doughnut charts for hierarchical data by using multiple `Pie` series with `radius` and `innerRadius` values:

```typescript
<ejs-accumulationchart id="container">
  <e-accumulation-series-collection>
    <!-- Outer ring -->
    <e-accumulation-series
      [dataSource]="outerData"
      xName="x"
      yName="y"
      type="Pie"
      [radius]="'60%'"
      [innerRadius]="'40%'>
    </e-accumulation-series>
    <!-- Inner ring -->
    <e-accumulation-series
      [dataSource]="innerData"
      xName="x"
      yName="y"
      type="Pie"
      [radius]="'35%'"
      [innerRadius]="'15%'>
    </e-accumulation-series>
  </e-accumulation-series-collection>
</ejs-accumulationchart>
```

## Pyramid Chart

### Basic Pyramid Chart

Pyramid charts display hierarchical data in descending order from top to bottom.

```typescript
<e-accumulation-series-collection>
  <e-accumulation-series
    [dataSource]="pyramidData"
    xName="x"
    yName="y"
    type="Pyramid">
  </e-accumulation-series>
</e-accumulation-series-collection>
```

### Pyramid Configuration

```typescript
<e-accumulation-series-collection>
  <e-accumulation-series
    [dataSource]="data"
    xName="stage"
    yName="count"
    type="Pyramid"
    [pyramidMode]="'Linear'"
    [neckWidth]="'20%']">
  </e-accumulation-series>
</e-accumulation-series-collection>
```

#### neckHeight
Defines the height of the pyramid neck region.

**API:** `neckHeight: string`

```html
<e-accumulation-series
  type="Pyramid"
  neckHeight="30%">
</e-accumulation-series>
```

**Pyramid Mode Options:**
- `Linear` - Standard pyramid shape
- `Surface` - Proportional by surface area

### Pyramid Chart Example

```typescript
@Component({
  template: `
    <ejs-accumulationchart id="container" [title]="'Sales Funnel'">
      <e-accumulation-series
        [dataSource]="funnelData"
        xName="stage"
        yName="leads"
        type="Pyramid"
        [dataLabel]="{ visible: true, position: 'Inside', name: 'stage' }">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `,
  styles: [`#container { height: 420px; }`]
})
export class PyramidComponent {
  funnelData = [
    { stage: 'Total Leads', leads: 5000 },
    { stage: 'Qualified', leads: 3500 },
    { stage: 'Proposal', leads: 2100 },
    { stage: 'Negotiation', leads: 1200 },
    { stage: 'Closed', leads: 600 }
  ];
}
```

## Funnel Chart

### Basic Funnel Chart

Funnel charts are similar to pyramids but typically used for conversion/drop-off data.

```typescript
  <e-accumulation-series-collection>
    <e-accumulation-series
      [dataSource]="conversionData"
      xName="stage"
      yName="users"
      type="Funnel">
    </e-accumulation-series>
  </e-accumulation-series-collection>
```

### Funnel vs Pyramid

- **Pyramid**: Hierarchical data, widest to narrowest
- **Funnel**: Conversion stages, emphasizing drop-off at each step
- **Visual**: Funnel can display non-linear progression

### Funnel Chart Example

```typescript
@Component({
  template: `
    <ejs-accumulationchart id="container" [title]="'User Conversion Funnel'">
      <e-accumulation-series-collection>
        <e-accumulation-series
          [dataSource]="conversionData"
          xName="stage"
          yName="users"
          type="Funnel"
          [dataLabel]="{ 
            visible: true, 
            position: 'Inside', 
            format: '${users}' 
          }">
        </e-accumulation-series>
      </e-accumulation-series-collection>
    </ejs-accumulationchart>
  `,
  styles: [`#container { height: 420px; }`]
})
export class FunnelComponent {
  conversionData = [
    { stage: 'Users Visited', users: 10000 },
    { stage: 'Product Viewed', users: 7500 },
    { stage: 'Added to Cart', users: 4200 },
    { stage: 'Checkout Started', users: 2500 },
    { stage: 'Purchase Complete', users: 1500 }
  ];
}
```

## Comparing Chart Types

### Type Selection Guide

```
Need part-to-whole? → Pie or Doughnut
├─ Want center label/metric? → Doughnut
└─ Simple 2D pie? → Pie

Need hierarchical flow? → Pyramid or Funnel
├─ Fixed hierarchy (e.g., organizational)? → Pyramid
└─ Conversion/drop-off stages? → Funnel
```

### Quick Comparison Table

| Feature | Pie | Doughnut | Pyramid | Funnel |
|---------|-----|----------|---------|--------|
| Center Content | No | Yes | No | No |
| Part-to-whole | Yes | Yes | No | No |
| Hierarchical | No | No | Yes | Yes |
| Conversion Data | No | No | Possible | Yes |
| Nesting Possible | No | Yes | No | No |

## Multiple Series

### Multiple Pie/Doughnut Series

Render multiple accumulation series in one chart:

```typescript
<ejs-accumulationchart id="container">
  <e-accumulation-series-collection>
    <!-- Series 1: This Year (Pie with innerRadius) -->
    <e-accumulation-series
      [dataSource]="thisYearData"
      xName="product"
      yName="sales"
      type="Pie"
      [radius]="'40%'>
    </e-accumulation-series>
    
    <!-- Series 2: Last Year (Pie with innerRadius) -->
    <e-accumulation-series
      [dataSource]="lastYearData"
      xName="product"
      yName="sales"
      type="Pie"
      [radius]="'100%'"
      [innerRadius]="'50%']>
    </e-accumulation-series>
  </e-accumulation-series-collection>
</ejs-accumulationchart>
```

**Use Cases:**
- Compare data across time periods
- Show breakdowns at different levels
- Side-by-side comparisons

## Series Properties

### Common Series Properties

```typescript
interface AccumulationSeriesProperties {
  dataSource: any[];           // Array of data objects
  xName: string;               // Property name for categories
  yName: string;               // Property name for values
  type: 'Pie' | 'Pyramid' | 'Funnel'; // Chart type (use `innerRadius` with `Pie` to create a doughnut)
  radius?: string | number;    // Series radius ('100%', 200, etc.)
  innerRadius?: string;        // Inner radius for doughnut
  startAngle?: number;         // Starting angle (0-360)
  endAngle?: number;           // Ending angle (0-360)
  explode?: boolean;           // Enable point separation
  explodeIndex?: number;       // Index of point to explode
  emptyPointSettings?: object; // Handle null/zero values
  dataLabel?: object;          // Label configuration
  tooltip?: object;            // Tooltip settings
  animation?: object;          // Animation settings
  cornerRadius?: string;       // Border radius for segments
}
```

### Type-Specific Properties

**Doughnut (via innerRadius):**
```typescript
{
  innerRadius: '40%'    // Hollow center size (use with type: 'Pie')
}
```

**Pyramid/Funnel:**
```typescript
{
  pyramidMode: 'Linear',    // 'Linear' or 'Surface' (Pyramid only)
  neckWidth: '20%',         // Width at narrowest point
  gapRatio: 0.1             // Gap between segments
}
```

### Styling Series

```typescript
<e-accumulation-series
  [dataSource]="data"
  xName="x"
  yName="y"
  type="Pie"
  [pointRender]="onPointRender">
</e-accumulation-series>

onPointRender(args: IAccumulationEventArgs) {
  // Customize color, border, etc. per point
  if (args.pointIndex === 0) {
    args.fill = '#FF6B6B';
    args.border = { color: '#C92A2A', width: 2 };
  }
}
```

## Key Takeaways

- **Pie vs Doughnut**: Pie for simple data, Doughnut for center labels
- **Pyramid vs Funnel**: Pyramid for hierarchy, Funnel for conversion
- **Multiple Series**: Layer different series for comparisons
- **Customize**: Use type-specific properties to match your data needs
- **Dynamic Update**: Change data and type at runtime; chart adapts automatically

---

## API Reference Summary

### Series Type APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `type` | Chart type (Pie, Doughnut, Pyramid, Funnel) | [type](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#type) |
| `radius` | Series radius | [radius](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#radius) |
| `innerRadius` | Inner radius for doughnut | [innerRadius](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#innerradius) |
| `startAngle` | Starting angle (0-360) | [startAngle](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#startangle) |
| `endAngle` | Ending angle (0-360) | [endAngle](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#endangle) |
| `explode` | Enable point explosion | [explode](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#explode) |
| `explodeIndex` | Index of point to explode | [explodeIndex](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#explodeindex) |
| `pyramidMode` | Pyramid shape mode (Linear/Surface) | [pyramidMode](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#pyramidmode) |
| `neckWidth` | Pyramid neck width | [neckWidth](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#neckwidth) |
| `gapRatio` | Gap between segments | [gapRatio](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#gapratio) |

### Enumerations

| Enum | Values | Documentation Link |
|------|--------|-------------------|
| `AccumulationType` | Pie, Doughnut, Pyramid, Funnel | [AccumulationType](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationType) |
| `PyramidMode` | Linear, Surface | [PyramidMode](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/pyramidMode) |

**For complete API documentation, see:** [api-reference.md](references/api-reference.md)