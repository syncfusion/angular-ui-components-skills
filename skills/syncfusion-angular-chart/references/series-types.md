# Chart Series Types

The Syncfusion Angular Chart component supports 25+ series types for visualizing different kinds of data. This guide covers all available series types, their use cases, and implementation examples.

## Table of Contents

- [Overview](#overview)
- [Line Series](#line-series)
  - [Line](#line)
  - [Step Line](#step-line)
  - [Spline](#spline)
  - [Stacked Line](#stacked-line)
  - [Multi-colored Line](#multi-colored-line)
- [Area Series](#area-series)
  - [Area](#area)
  - [Stacked Area](#stacked-area)
  - [100% Stacked Area](#100-stacked-area)
  - [Range Area](#range-area)
  - [Spline Range Area](#spline-range-area)
- [Column and Bar Series](#column-and-bar-series)
  - [Column](#column)
  - [Bar](#bar)
  - [Stacked Column/Bar](#stacked-columnbar)
  - [100% Stacked Column/Bar](#100-stacked-columnbar)
  - [Range Column](#range-column)
- [Financial Series](#financial-series)
  - [Candlestick](#candlestick)
  - [Hilo](#hilo)
  - [HiloOpenClose](#hiloopenclose)
- [Statistical Series](#statistical-series)
  - [Box and Whisker](#box-and-whisker)
  - [Histogram](#histogram)
  - [Pareto](#pareto)
  - [Error Bar](#error-bar)
- [Specialized Series](#specialized-series)
  - [Scatter](#scatter)
  - [Bubble](#bubble)
  - [Polar](#polar)
  - [Radar](#radar)
  - [Waterfall](#waterfall)
  - [Vertical Chart](#vertical-chart)
- [Choosing the Right Series Type](#choosing-the-right-series-type)
- [Combining Multiple Series](#combining-multiple-series)

## Overview

Each series type is optimized for specific data visualization scenarios. The `type` property on the `<e-series>` element determines which series type to render.

**API Reference:** 
- [Series.type](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#type) - ChartSeriesType enum with all available series types
- [ChartSeriesType](https://ej2.syncfusion.com/angular/documentation/api/chart/chartSeriesType) - Enum listing all 25+ series types
- [SeriesDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective) - Complete series configuration properties

**Basic Syntax:**
```html
<e-series [dataSource]="data" type="Line" xName="x" yName="y"></e-series>
```

**Common Series Properties:**

| Property | Type | Default | Description | API Reference |
|----------|------|---------|-------------|---------------|
| `type` | ChartSeriesType | 'Line' | Series type (Line, Column, Bar, Area, etc.) | [Series.type](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#type) |
| `dataSource` | Object[] | '' | Series data array or DataManager | [Series.dataSource](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#dataSource) |
| `xName` | string | '' | X-axis field name | [Series.xName](https://ej2.syncfusion.com/angular/documentation/api/chart/series#xName) |
| `yName` | string | '' | Y-axis field name | [Series.yName](https://ej2.syncfusion.com/angular/documentation/api/chart/series#yName) |
| `name` | string | '' | Series name for legend | [Series.name](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#name) |
| `fill` | string | null | Series fill color | [Series.fill](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#fill) |
| `width` | number | 2 | Line/border width | [Series.width](https://ej2.syncfusion.com/angular/documentation/api/chart/series#width) |
| `opacity` | number | 1 | Series opacity (0-1) | [Series.opacity](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#opacity) |
| `marker` | MarkerSettingsModel | - | Data point markers | [MarkerSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettingsModel) |

## Line Series

Line series visualize trends and changes over continuous data.

### Line

Classic line chart connecting data points with straight lines.

**Use Case:** Trend analysis, time-series data, continuous data tracking

**Example:**
```typescript
import { Component } from '@angular/core';
import { ChartModule } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-line-chart',
  standalone: true,
  imports: [ChartModule],
  template: `
    <ejs-chart [primaryXAxis]="primaryXAxis">
      <e-series-collection>
        <e-series [dataSource]="data" type="Line" xName="month" yName="sales" 
                  name="Sales" width="2" [marker]="marker">
        </e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class LineChartComponent {
  public data = [
    { month: 'Jan', sales: 35 },
    { month: 'Feb', sales: 28 },
    { month: 'Mar', sales: 34 },
    { month: 'Apr', sales: 32 },
    { month: 'May', sales: 40 }
  ];
  
  public primaryXAxis = { valueType: 'Category' };
  public marker = { visible: true, width: 10, height: 10 };
}
```

### Step Line

Connects points with horizontal and vertical segments, emphasizing discrete changes.

**Use Case:** Step functions, discrete value changes, inventory levels

**Example:**
```typescript
<e-series [dataSource]="data" type="StepLine" xName="x" yName="y" width="2"></e-series>
```

### Spline

Smoothed line using spline interpolation for elegant curves.

**Use Case:** Smooth trend visualization, predictions, aesthetic presentations

**Example:**
```typescript
public splineData = [
  { x: 'Jan', y: 35 }, { x: 'Feb', y: 28 }, { x: 'Mar', y: 34 },
  { x: 'Apr', y: 32 }, { x: 'May', y: 40 }, { x: 'Jun', y: 32 }
];
```

```html
<e-series [dataSource]="splineData" type="Spline" xName="x" yName="y" width="2"></e-series>
```

### Stacked Line

Multiple line series stacked vertically to show cumulative values.

**Use Case:** Cumulative trends, part-to-whole over time

**Example:**
```typescript
public data1 = [{ x: 'Jan', y: 10 }, { x: 'Feb', y: 20 }, { x: 'Mar', y: 15 }];
public data2 = [{ x: 'Jan', y: 15 }, { x: 'Feb', y: 10 }, { x: 'Mar', y: 20 }];
public data3 = [{ x: 'Jan', y: 20 }, { x: 'Feb', y: 15 }, { x: 'Mar', y: 18 }];
```

```html
<e-series-collection>
  <e-series [dataSource]="data1" type="StackingLine" xName="x" yName="y" name="Product A"></e-series>
  <e-series [dataSource]="data2" type="StackingLine" xName="x" yName="y" name="Product B"></e-series>
  <e-series [dataSource]="data3" type="StackingLine" xName="x" yName="y" name="Product C"></e-series>
</e-series-collection>
```

### Multi-colored Line

Line segments colored based on value ranges or conditions.

**Use Case:** Highlighting thresholds, color-coding performance zones

**Example:**
```typescript
public data = [
  { x: 'Jan', y: 35, color: 'green' },
  { x: 'Feb', y: 28, color: 'red' },
  { x: 'Mar', y: 34, color: 'green' },
  { x: 'Apr', y: 32, color: 'orange' }
];
```

```html
<e-series [dataSource]="data" type="MultiColoredLine" xName="x" yName="y" 
          pointColorMapping="color" width="3">
</e-series>
```

## Area Series

Area series show magnitude with filled regions beneath lines.

### Area

Fills the area between the line and axis.

**Use Case:** Volume visualization, magnitude emphasis, filled trends

**Example:**
```typescript
<e-series [dataSource]="data" type="Area" xName="x" yName="y" 
          name="Sales" opacity="0.5" fill="#E94649">
</e-series>
```

### Stacked Area

Multiple area series stacked to show cumulative contribution.

**Use Case:** Part-to-whole trends, market share over time

**Example:**
```html
<e-series-collection>
  <e-series [dataSource]="data1" type="StackingArea" xName="x" yName="y" name="Product A"></e-series>
  <e-series [dataSource]="data2" type="StackingArea" xName="x" yName="y" name="Product B"></e-series>
  <e-series [dataSource]="data3" type="StackingArea" xName="x" yName="y" name="Product C"></e-series>
</e-series-collection>
```

### 100% Stacked Area

Normalized stacked areas scaled to 100% for proportional comparison.

**Use Case:** Percentage composition over time, relative proportions

**Example:**
```html
<e-series-collection>
  <e-series [dataSource]="data1" type="StackingArea100" xName="x" yName="y" name="Desktop"></e-series>
  <e-series [dataSource]="data2" type="StackingArea100" xName="x" yName="y" name="Mobile"></e-series>
  <e-series [dataSource]="data3" type="StackingArea100" xName="x" yName="y" name="Tablet"></e-series>
</e-series-collection>
```

### Range Area

Displays min-max value ranges as filled areas.

**Use Case:** Temperature ranges, confidence intervals, variability bands

**Example:**
```typescript
public rangeData = [
  { x: 'Jan', low: 15, high: 25 },
  { x: 'Feb', low: 17, high: 28 },
  { x: 'Mar', low: 20, high: 32 }
];
```

```html
<e-series [dataSource]="rangeData" type="RangeArea" 
          xName="x" low="low" high="high" opacity="0.4">
</e-series>
```

### Spline Range Area

Smooth (spline) version of range area for refined presentation.

**Example:**
```html
<e-series [dataSource]="rangeData" type="SplineRangeArea" 
          xName="x" low="low" high="high">
</e-series>
```

## Column and Bar Series

Vertical (column) and horizontal (bar) series for categorical comparisons.

### Column

Vertical bars for straightforward category comparison.

**Use Case:** Category comparison, period-over-period analysis

**API Properties:**
- [Series.type](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#type) = 'Column'
- [Series.columnWidth](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#columnWidth) - Column width (default: null, auto-calculated as 0.7)
- [Series.columnSpacing](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#columnSpacing) - Space between columns (default: 0, range: 0-1)
- [Series.columnFacet](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#columnFacet) - Column shape: 'Rectangle' or 'Cylinder' (default: 'Rectangle')
- [Series.columnWidthInPixel](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#columnWidthInPixel) - Column width in pixels (default: null)
- [Series.cornerRadius](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#cornerRadius) - Rounded corners configuration ([CornerRadiusModel](https://ej2.syncfusion.com/angular/documentation/api/chart/cornerRadiusModel))
- [Series.border](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#border) - Column border settings ([BorderModel](https://ej2.syncfusion.com/angular/documentation/api/chart/borderModel))

**Example:**
```typescript
public columnData = [
  { country: 'USA', sales: 50 },
  { country: 'China', sales: 65 },
  { country: 'India', sales: 45 },
  { country: 'Japan', sales: 40 }
];
```

```html
<e-series [dataSource]="columnData" type="Column" 
          xName="country" yName="sales" name="Sales">
</e-series>
```

### Bar

Horizontal bars, ideal for long category labels.

**Use Case:** Rankings, long labels, horizontal comparisons

**Example:**
```html
<e-series [dataSource]="columnData" type="Bar" 
          xName="country" yName="sales">
</e-series>
```

### Stacked Column/Bar

Multiple series stacked within single bars to show component contributions.

**Use Case:** Component breakdown, segment analysis

**Example:**
```html
<e-series-collection>
  <e-series [dataSource]="q1Data" type="StackingColumn" xName="month" yName="sales" name="Q1"></e-series>
  <e-series [dataSource]="q2Data" type="StackingColumn" xName="month" yName="sales" name="Q2"></e-series>
  <e-series [dataSource]="q3Data" type="StackingColumn" xName="month" yName="sales" name="Q3"></e-series>
</e-series-collection>
```

**For horizontal stacking:**
```html
<e-series type="StackingBar" ...></e-series>
```

### 100% Stacked Column/Bar

Normalized to 100% for proportional comparison.

**Example:**
```html
<e-series [dataSource]="data1" type="StackingColumn100" xName="x" yName="y"></e-series>
<e-series [dataSource]="data2" type="StackingColumn100" xName="x" yName="y"></e-series>
```

### Range Column

Shows high-low value ranges as columns.

**Use Case:** Temperature ranges, price ranges, min-max comparisons

**Example:**
```typescript
public rangeColumnData = [
  { x: 'Jan', low: 10, high: 20 },
  { x: 'Feb', low: 15, high: 30 },
  { x: 'Mar', low: 18, high: 35 }
];
```

```html
<e-series [dataSource]="rangeColumnData" type="RangeColumn" 
          xName="x" low="low" high="high">
</e-series>
```

## Financial Series

Specialized series for financial and stock market data.

**API Reference:** [FinancialDataFields](https://ej2.syncfusion.com/angular/documentation/api/chart/financialDataFields) - Interface for OHLC data fields

### Candlestick

OHLC bars with color coding for price direction (bullish/bearish).

**Use Case:** Stock price analysis, OHLC data visualization

**API Properties:**
- [Series.type](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#type) = 'Candle'
- [Series.open](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#open) - Open price field name
- [Series.high](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#high) - High price field name  
- [Series.low](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#low) - Low price field name
- [Series.close](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#close) - Close price field name
- [Series.bearFillColor](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#bearFillColor) - Color for bearish candles (default: null)
- [Series.bullFillColor](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#bullFillColor) - Color for bullish candles (default: null)
- [Series.enableSolidCandles](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#enableSolidCandles) - Enable solid candle rendering (default: false)

**Example:**
```typescript
public stockData = [
  { date: new Date(2024, 1, 1), open: 120, high: 135, low: 110, close: 130 },
  { date: new Date(2024, 1, 2), open: 130, high: 145, low: 125, close: 140 },
  { date: new Date(2024, 1, 3), open: 140, high: 150, low: 135, close: 138 },
  { date: new Date(2024, 1, 4), open: 138, high: 142, low: 128, close: 135 }
];

public primaryXAxis = { valueType: 'DateTime' };
```

```html
<ejs-chart [primaryXAxis]="primaryXAxis">
  <e-series-collection>
    <e-series [dataSource]="stockData" type="Candle" 
              xName="date" open="open" high="high" low="low" close="close"
              bearFillColor="#e74c3c" bullFillColor="#2ecc71">
    </e-series>
  </e-series-collection>
</ejs-chart>
```

### Hilo

Shows only high and low values per period for compact volatility view.

**Use Case:** Price volatility, range visualization

**Example:**
```html
<e-series [dataSource]="stockData" type="Hilo" 
          xName="date" high="high" low="low">
</e-series>
```

### HiloOpenClose

Full OHLC representation with open/close markers.

**Example:**
```html
<e-series [dataSource]="stockData" type="HiloOpenClose" 
          xName="date" open="open" high="high" low="low" close="close">
</e-series>
```

## Statistical Series

Series types for statistical analysis and distributions.

### Box and Whisker

Displays distribution with median, quartiles, and outliers.

**Use Case:** Statistical comparison, distribution analysis, outlier detection

**Example:**
```typescript
public boxData = [
  { x: 'Department A', y: [22, 22, 23, 25, 25, 25, 26, 27, 27, 28, 28, 29, 30, 32, 34, 35, 37, 38, 41, 43] },
  { x: 'Department B', y: [24, 25, 25, 26, 27, 28, 29, 30, 34, 36, 38] }
];
```

```html
<e-series [dataSource]="boxData" type="BoxAndWhisker" 
          xName="x" yName="y" [marker]="marker">
</e-series>
```

### Histogram

Displays frequency distribution of numeric data bins.

**Use Case:** Distribution shape, frequency analysis, data spread

**API Properties:**
- [Series.type](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#type) = 'Histogram'
- [Series.binInterval](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#binInterval) - Histogram bin width (default: null, auto-calculated)
- [Series.showNormalDistribution](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#showNormalDistribution) - Show normal distribution curve (default: false)
- [Series.columnWidth](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#columnWidth) - Column width (default: 1 for histogram)

**Example:**
```typescript
public histogramData = [
  { value: 5.250 }, { value: 7.750 }, { value: 8.275 },
  { value: 9.750 }, { value: 7.750 }, { value: 8.275 },
  { value: 6.250 }, { value: 5.750 }, { value: 8.275 }
];
```

```html
<e-series [dataSource]="histogramData" type="Histogram" 
          yName="value" binInterval="1" showNormalDistribution="true">
</e-series>
```

### Pareto

Bar chart plus cumulative line for 80/20 analysis.

**Use Case:** Quality control, identifying vital few, prioritization

**Example:**
```typescript
public paretoData = [
  { defectType: 'Defect A', count: 40 },
  { defectType: 'Defect B', count: 30 },
  { defectType: 'Defect C', count: 15 },
  { defectType: 'Defect D', count: 10 },
  { defectType: 'Defect E', count: 5 }
];
```

```html
<e-series [dataSource]="paretoData" type="Pareto" 
          xName="defectType" yName="count" name="Defects">
</e-series>
```

### Error Bar

Adds error/uncertainty ranges to data points.

**Use Case:** Measurement uncertainty, confidence intervals, variance display

**Example:**
```typescript
public errorBarSettings = {
  visible: true,
  type: 'Fixed',
  verticalError: 3
};
```

```html
<e-series [dataSource]="data" type="Line" xName="x" yName="y" 
          [errorBar]="errorBarSettings">
</e-series>
```

## Specialized Series

Unique series types for specific visualization needs.

### Scatter

Individual x/y points showing correlation or clusters.

**Use Case:** Correlation analysis, cluster identification, relationship visualization

**Example:**
```typescript
public scatterData = [
  { height: 160, weight: 55 },
  { height: 165, weight: 62 },
  { height: 170, weight: 68 },
  { height: 175, weight: 75 },
  { height: 180, weight: 80 }
];
```

```html
<e-series [dataSource]="scatterData" type="Scatter" 
          xName="height" yName="weight" [marker]="marker">
</e-series>
```

### Bubble

Scatter with point size encoding a third dimension.

**Use Case:** Three-variable visualization, size-encoded data

**Example:**
```typescript
public bubbleData = [
  { x: 'USA', y: 50, size: 100 },
  { x: 'China', y: 65, size: 150 },
  { x: 'India', y: 45, size: 80 }
];
```

```html
<e-series [dataSource]="bubbleData" type="Bubble" 
          xName="x" yName="y" size="size" minRadius="3" maxRadius="8">
</e-series>
```

### Polar

Circular chart with radial axes from center.

**Use Case:** Cyclical data, directional data, circular patterns

**Example:**
```html
<ejs-chart [primaryXAxis]="primaryXAxis">
  <e-series-collection>
    <e-series [dataSource]="data" type="Polar" xName="x" yName="y" 
              drawType="Line">
    </e-series>
  </e-series-collection>
</ejs-chart>
```

**drawType options:** Line, Column, Area, Scatter, Spline, StackingArea, StackingColumn

### Radar

Similar to polar but with straight lines from center.

**Use Case:** Multi-variable comparison, skill assessments, radar plots

**Example:**
```html
<e-series [dataSource]="data" type="Radar" xName="x" yName="y" 
          drawType="Line">
</e-series>
```

### Waterfall

Shows sequential positive/negative contributions to cumulative total.

**Use Case:** Financial reconciliations, cumulative effects, bridge charts

**Example:**
```typescript
public waterfallData = [
  { x: 'Starting', y: 100 },
  { x: 'Sales', y: 50 },
  { x: 'Costs', y: -30 },
  { x: 'Marketing', y: -15 },
  { x: 'Ending', y: 105 }
];

public connector = { color: '#333', width: 1 };
```

```html
<e-series [dataSource]="waterfallData" type="Waterfall" 
          xName="x" yName="y" intermediateSumIndexes="[3]" 
          sumIndexes="[4]" [connector]="connector">
</e-series>
```

### Vertical Chart

Inverts chart orientation (rotates axes).

**Use Case:** Alternative perspective, specific layout requirements

**Example:**
```html
<ejs-chart isTransposed="true">
  <e-series-collection>
    <e-series [dataSource]="data" type="Column" xName="x" yName="y"></e-series>
  </e-series-collection>
</ejs-chart>
```

## Choosing the Right Series Type

| Data Type | Recommended Series |
|-----------|-------------------|
| **Trend over time** | Line, Spline, Area |
| **Category comparison** | Column, Bar |
| **Part-to-whole** | Stacked Column, Stacked Area, 100% Stacked |
| **Distribution** | Histogram, Box & Whisker |
| **Correlation** | Scatter, Bubble |
| **Financial/Stock** | Candlestick, Hilo, HiloOpenClose |
| **Range/Min-Max** | Range Area, Range Column |
| **Prioritization** | Pareto |
| **Sequential changes** | Waterfall |
| **Cyclical patterns** | Polar, Radar |

## Combining Multiple Series

Mix different series types in one chart for rich visualizations:

```typescript
public data1 = [{ x: 'Jan', y: 35 }, { x: 'Feb', y: 28 }];
public data2 = [{ x: 'Jan', y: 40 }, { x: 'Feb', y: 45 }];
public data3 = [{ x: 'Jan', y: 30 }, { x: 'Feb', y: 25 }];
```

```html
<ejs-chart>
  <e-series-collection>
    <!-- Column series for actuals -->
    <e-series [dataSource]="data1" type="Column" xName="x" yName="y" name="Actual"></e-series>
    
    <!-- Line series for target -->
    <e-series [dataSource]="data2" type="Line" xName="x" yName="y" name="Target" width="2"></e-series>
    
    <!-- Area series for baseline -->
    <e-series [dataSource]="data3" type="Area" xName="x" yName="y" name="Baseline" opacity="0.3"></e-series>
  </e-series-collection>
</ejs-chart>
```

**Best Practices:**
- Limit to 2-3 different series types per chart
- Ensure visual hierarchy (emphasize primary data)
- Use consistent color schemes
- Consider axis scaling when combining types

## Common Pitfalls

1. **Wrong Series for Data Type:** Using line charts for categorical comparisons
2. **Too Many Series:** Cluttered charts with 5+ series
3. **Inconsistent Data Structure:** Different data formats across series
4. **Missing Required Properties:** Forgetting `low`/`high` for range series, `open`/`close` for financial

## Performance Considerations

- **Large Datasets:** Use `enableCanvas` rendering for 10,000+ points
- **Many Series:** Limit to 10-15 series per chart
- **Animation:** Disable for real-time updates: `animation: { enable: false }`

Refer to the data-binding reference for optimization techniques.

## API Reference Summary

### Core Series Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| ChartSeriesType | Enum defining all 25+ series types | [chartSeriesType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartSeriesType) |
| SeriesDirective | Main series configuration with 60+ properties | [seriesDirective.md](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective) |
| Series | Series class with methods and properties | [series.md](https://ej2.syncfusion.com/angular/documentation/api/chart/series) |
| SeriesModel | Series interface/model definition | [seriesModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesModel) |

### Series Type-Specific Properties

| Series Type | Key Properties | API Reference |
|-------------|----------------|---------------|
| **Line/Spline** | width, dashArray, splineType | [Series.width](https://ej2.syncfusion.com/angular/documentation/api/chart/series#width), [SplineType](https://ej2.syncfusion.com/angular/documentation/api/chart/splineType) |
| **Column/Bar** | columnWidth, columnSpacing, columnFacet, cornerRadius | [Series.columnWidth](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#columnWidth), [ColumnFacet](https://ej2.syncfusion.com/angular/documentation/api/chart/columnFacet) |
| **Area** | opacity, fill, border | [Series.opacity](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#opacity), [BorderModel](https://ej2.syncfusion.com/angular/documentation/api/chart/borderModel) |
| **Financial** | open, high, low, close, bearFillColor, bullFillColor, enableSolidCandles | [FinancialDataFields](https://ej2.syncfusion.com/angular/documentation/api/chart/financialDataFields) |
| **Range** | low, high | [Series.low](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#low), [Series.high](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#high) |
| **Bubble** | size, minRadius, maxRadius | [Series.size](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#size) |
| **Polar/Radar** | drawType, isClosed | [ChartDrawType](https://ej2.syncfusion.com/angular/documentation/api/chart/chartDrawType), [Series.isClosed](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#isClosed) |
| **Waterfall** | intermediateSumIndexes, sumIndexes, connector | [Series.connector](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#connector), [ConnectorModel](https://ej2.syncfusion.com/angular/documentation/api/chart/connectorModel) |
| **Box & Whisker** | showMean, showOutliers, boxPlotMode | [Series.showMean](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#showMean), [BoxPlotMode](https://ej2.syncfusion.com/angular/documentation/api/chart/boxPlotMode) |
| **Histogram** | binInterval, showNormalDistribution | [Series.binInterval](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#binInterval) |
| **Pareto** | paretoOptions | [ParetoOptions](https://ej2.syncfusion.com/angular/documentation/api/chart/paretoOptions), [ParetoOptionsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/paretoOptionsModel) |

### Styling and Appearance

| Property | Type | Default | API Reference |
|----------|------|---------|---------------|
| fill | string | null | [Series.fill](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#fill) |
| opacity | number | 1 | [Series.opacity](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#opacity) |
| border | BorderModel | - | [BorderModel](https://ej2.syncfusion.com/angular/documentation/api/chart/borderModel) |
| dashArray | string | '' | [Series.dashArray](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#dashArray) |
| animation | AnimationModel | - | [AnimationModel](https://ej2.syncfusion.com/angular/documentation/api/chart/animationModel) |

### Data Point Customization

| Feature | API Reference |
|---------|---------------|
| Markers | [MarkerSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettingsModel) |
| Data Labels | [DataLabelSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelSettingsModel) |
| Point Colors | [RangeColorSettingModel](https://ej2.syncfusion.com/angular/documentation/api/chart/rangeColorSettingModel) |
| Empty Points | [EmptyPointSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/emptyPointSettingsModel) |

### Enumerations

| Enum | Description | API Reference |
|------|-------------|---------------|
| ChartSeriesType | All series type values | [chartSeriesType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartSeriesType) |
| ChartDrawType | Draw types for Polar/Radar | [chartDrawType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartDrawType) |
| SplineType | Spline interpolation types | [splineType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/splineType) |
| BoxPlotMode | Box plot calculation modes | [boxPlotMode.md](https://ej2.syncfusion.com/angular/documentation/api/chart/boxPlotMode) |
