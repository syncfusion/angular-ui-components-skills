# Axes and Chart Layout

Axes define how data maps to visual space and provide context through labels, gridlines, and titles. This guide covers axis configuration, multiple axes, layout options, and chart structure.

## Table of Contents

- [Axis Types](#axis-types)
- [Primary and Secondary Axes](#primary-and-secondary-axes)
- [Multiple Axes](#multiple-axes)
- [Axis Labels](#axis-labels)
- [Gridlines and Tick Lines](#gridlines-and-tick-lines)
- [Axis Crossing and Inversion](#axis-crossing-and-inversion)
- [Multiple Panes](#multiple-panes)
- [Chart Title and Subtitle](#chart-title-and-subtitle)
- [Chart Dimensions](#chart-dimensions)
- [Margins and Padding](#margins-and-padding)

## Axis Types

The Syncfusion Chart supports four axis value types for mapping different data formats.

**API Reference:** 
- [AxisModel](https://ej2.syncfusion.com/angular/documentation/api/chart/axisModel) - Complete axis configuration with 50+ properties
- [Axis](https://ej2.syncfusion.com/angular/documentation/api/chart/axis) - Axis class documentation
- [ValueType](https://ej2.syncfusion.com/angular/documentation/api/chart/valueType) - Enum for axis value types

### Numeric Axis (Double)

For continuous numerical data (default type).

**API Properties:**
- [Axis.valueType](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#valueType) = 'Double' (default)
- [Axis.minimum](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#minimum) (Object, default: null) - Minimum axis value
- [Axis.maximum](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#maximum) (Object, default: null) - Maximum axis value
- [Axis.interval](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#interval) (number, default: null) - Label interval spacing
- [Axis.rangePadding](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#rangePadding) - [ChartRangePadding](https://ej2.syncfusion.com/angular/documentation/api/chart/chartRangePadding) enum (None, Normal, Additional, Round, Auto)
- [Axis.desiredIntervals](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#desiredIntervals) (number, default: null) - Desired number of intervals

```typescript
public primaryYAxis = {
  valueType: 'Double',  // Default, can be omitted
  minimum: 0,
  maximum: 100,
  interval: 20,
  rangePadding: 'Normal',
  title: 'Sales (in USD)'
};
```

### DateTime Axis

For time-series data with dates.

**API Properties:**
- [Axis.valueType](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#valueType) = 'DateTime'
- [Axis.labelFormat](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelFormat) (string, default: '') - Date/time format string
- [Axis.intervalType](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#intervalType) - [IntervalType](https://ej2.syncfusion.com/angular/documentation/api/chart/intervalType) enum (Years, Months, Days, Hours, Minutes, Seconds, Auto)
- [Axis.interval](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#interval) (number, default: null) - Number of intervals
- [Axis.skeleton](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#skeleton) (string, default: '') - Skeleton format for date labels
- [Axis.skeletonType](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#skeletonType) - [SkeletonType](https://ej2.syncfusion.com/angular/documentation/api/chart/skeletonType) enum (DateTime, Date, Time)

```typescript
public data = [
  { date: new Date(2024, 0, 1), sales: 35 },
  { date: new Date(2024, 1, 1), sales: 28 },
  { date: new Date(2024, 2, 1), sales: 34 }
];

public primaryXAxis = {
  valueType: 'DateTime',
  labelFormat: 'MMM yyyy',
  intervalType: 'Months',
  interval: 1,
  skeleton: 'yMMM',
  title: 'Date'
};
```

**Interval Types:** See [IntervalType](https://ej2.syncfusion.com/angular/documentation/api/chart/intervalType)
- `Years`, `Months`, `Days`, `Hours`, `Minutes`, `Seconds`, `Auto`

**Label Formats:**
- `'dd/MM/yyyy'`: 01/03/2024
- `'MMM yyyy'`: Mar 2024
- `'HH:mm'`: 14:30

### Category Axis

For discrete categories (strings or numbers treated as categories).

```typescript
public data = [
  { category: 'Product A', sales: 35 },
  { category: 'Product B', sales: 28 },
  { category: 'Product C', sales: 34 }
];

public primaryXAxis = {
  valueType: 'Category',
  title: 'Products'
};
```

**Features:**
- Automatically spaces categories evenly
- Supports label rotation for long names
- Ideal for bar/column charts

### Logarithmic Axis

For data spanning multiple orders of magnitude.

**API Properties:**
- [Axis.valueType](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#valueType) = 'Logarithmic'
- [Axis.logBase](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#logBase) (number, default: 10) - Base of logarithm (e.g., 2, 10, e)
- [Axis.interval](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#interval) (number, default: null) - Interval in log scale

```typescript
public data = [
  { x: 1, y: 10 },
  { x: 2, y: 100 },
  { x: 3, y: 1000 },
  { x: 4, y: 10000 }
];

public primaryYAxis = {
  valueType: 'Logarithmic',
  logBase: 10,
  interval: 1,
  title: 'Value (log scale)'
};
```

**Use Cases:**
- Exponential growth data
- Scientific measurements
- Financial data with wide ranges

## Primary and Secondary Axes

Charts have primary X and Y axes by default. Secondary axes allow different scales.

### Primary Axes

```typescript
public primaryXAxis = {
  valueType: 'Category',
  title: 'Month'
};

public primaryYAxis = {
  title: 'Sales (USD)',
  minimum: 0,
  maximum: 100
};
```

```html
<ejs-chart [primaryXAxis]="primaryXAxis" [primaryYAxis]="primaryYAxis">
  <e-series-collection>
    <e-series [dataSource]="data" type="Column" xName="month" yName="sales"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Secondary Axes

Display series with different scales on opposite sides.

```typescript
public primaryYAxis = {
  title: 'Sales (USD)',
  minimum: 0,
  maximum: 100
};

public axes = [{
  name: 'secondaryY',
  opposedPosition: true,
  title: 'Temperature (°C)',
  minimum: 0,
  maximum: 50
}];
```

```html
<ejs-chart [primaryYAxis]="primaryYAxis" [axes]="axes">
  <e-series-collection>
    <e-series [dataSource]="salesData" type="Column" xName="month" yName="sales" name="Sales"></e-series>
    <e-series [dataSource]="tempData" type="Line" xName="month" yName="temp" 
              yAxisName="secondaryY" name="Temperature"></e-series>
  </e-series-collection>
</ejs-chart>
```

**Key Points:**
- Use `opposedPosition: true` to place axis on right/top
- Bind series to axis using `yAxisName` property
- Different axis types allowed (e.g., Double + DateTime)

## Multiple Axes

Create charts with more than two Y-axes for complex comparisons.

### Multiple Y-Axes

```typescript
public primaryYAxis = {
  name: 'yAxis1',
  title: 'Revenue (USD)',
  lineStyle: { width: 0 },
  majorTickLines: { width: 0 }
};

public axes = [
  {
    name: 'yAxis2',
    opposedPosition: true,
    title: 'Profit (%)',
    lineStyle: { width: 0 },
    majorTickLines: { width: 0 }
  },
  {
    name: 'yAxis3',
    rowIndex: 1,
    title: 'Units Sold',
    lineStyle: { width: 0 }
  }
];
```

```html
<e-series [dataSource]="revenue" type="Column" xName="x" yName="y" yAxisName="yAxis1"></e-series>
<e-series [dataSource]="profit" type="Line" xName="x" yName="y" yAxisName="yAxis2"></e-series>
<e-series [dataSource]="units" type="Area" xName="x" yName="y" yAxisName="yAxis3"></e-series>
```

### Multiple X-Axes

```typescript
public primaryXAxis = {
  name: 'xAxis1',
  valueType: 'Category'
};

public axes = [{
  name: 'xAxis2',
  opposedPosition: true,
  valueType: 'Category'
}];
```

```html
<e-series [dataSource]="data1" type="Column" xName="x" yName="y" xAxisName="xAxis1"></e-series>
<e-series [dataSource]="data2" type="Line" xName="x" yName="y" xAxisName="xAxis2"></e-series>
```

## Axis Labels

Customize label appearance, rotation, and formatting.

**API Reference:**
- [Axis.labelFormat](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelFormat) - Label format string
- [Axis.labelStyle](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelStyle) - [FontModel](https://ej2.syncfusion.com/angular/documentation/api/chart/fontModel) for label styling
- [Axis.labelRotation](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelRotation) - Label rotation angle
- [Axis.labelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelIntersectAction) - [LabelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/labelIntersectAction) enum
- [Axis.labelPlacement](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelPlacement) - [LabelPlacement](https://ej2.syncfusion.com/angular/documentation/api/chart/labelPlacement) enum
- [Axis.labelPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelPosition) - [AxisPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/axisPosition) enum

### Label Formatting

**API Property:** [Axis.labelFormat](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelFormat) (string, default: '')

```typescript
public primaryYAxis = {
  labelFormat: '${value}K',  // Format pattern
  title: 'Sales'
};
```

**Format Patterns:**
- `'${value}K'`: Append K (35K)
- `'${value}%'`: Percentage (35%)
- `'${value}M'`: Millions
- `'n2'`: Number with 2 decimals (35.00)
- `'c2'`: Currency with 2 decimals ($35.00)

**For DateTime:**
```typescript
public primaryXAxis = {
  valueType: 'DateTime',
  labelFormat: 'dd MMM'  // 01 Mar
};
```

### Label Rotation

Rotate labels to fit long text or save space.

**API Properties:**
- [Axis.labelRotation](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelRotation) (number, default: 0) - Rotation angle in degrees (-90 to 90)
- [Axis.labelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelIntersectAction) - [LabelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/labelIntersectAction) enum (default: 'Trim')

```typescript
public primaryXAxis = {
  valueType: 'Category',
  labelRotation: -45,  // Degrees (-90 to 90)
  labelIntersectAction: 'Rotate45'
};
```

**labelIntersectAction Options:** See [LabelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/labelIntersectAction)
- `None`: No action (may overlap)
- `Hide`: Hide overlapping labels
- `Trim`: Truncate with ellipsis
- `Wrap`: Wrap to multiple lines
- `MultipleRows`: Place in multiple rows
- `Rotate45`: Rotate 45 degrees
- `Rotate90`: Rotate 90 degrees

### Label Styling

**API Property:** [Axis.labelStyle](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelStyle) - [FontModel](https://ej2.syncfusion.com/angular/documentation/api/chart/fontModel)

```typescript
public primaryXAxis = {
  valueType: 'Category',
  labelStyle: {
    fontFamily: 'Arial',
    size: '14px',
    fontWeight: 'Bold',
    color: '#333'
  }
};
```

### Label Placement

```typescript
public primaryYAxis = {
  labelPlacement: 'OnTicks',  // BetweenTicks or OnTicks
  labelPosition: 'Outside',  // Inside or Outside
  title: 'Sales'
};
```

### Multilevel Labels

Group categories hierarchically.

```typescript
public primaryXAxis = {
  valueType: 'Category',
  multiLevelLabels: [
    {
      border: { type: 'Rectangle', color: '#333' },
      categories: [
        { start: 0, end: 2, text: 'Q1' },
        { start: 3, end: 5, text: 'Q2' },
        { start: 6, end: 8, text: 'Q3' },
        { start: 9, end: 11, text: 'Q4' }
      ]
    },
    {
      border: { type: 'Brace', color: '#666' },
      categories: [
        { start: 0, end: 5, text: 'H1' },
        { start: 6, end: 11, text: 'H2' }
      ]
    }
  ]
};
```

**Border Types:**
- `Rectangle`, `Brace`, `WithoutBorder`, `WithoutTopBorder`, `WithoutTopandBottomBorder`, `CurlyBrace`

### Custom Label Template

```html
<ejs-chart [primaryXAxis]="primaryXAxis">
  <ng-template #axisLabelTemplate let-data>
    <div style="background: #f0f0f0; padding: 2px 5px; border-radius: 3px;">
      {{data.value}}
    </div>
  </ng-template>
  <e-series-collection>
    <!-- series -->
  </e-series-collection>
</ejs-chart>
```

## Gridlines and Tick Lines

Customize gridlines and tick marks for better readability.

### Major Gridlines

```typescript
public primaryYAxis = {
  majorGridLines: {
    width: 1,
    color: '#e0e0e0',
    dashArray: '5,5'  // Dashed line pattern
  }
};
```

### Minor Gridlines

```typescript
public primaryYAxis = {
  minorGridLines: {
    width: 1,
    color: '#f0f0f0'
  },
  minorTicksPerInterval: 4  // Number of minor ticks between major ticks
};
```

### Tick Lines

```typescript
public primaryYAxis = {
  majorTickLines: {
    width: 2,
    height: 10,
    color: '#333'
  },
  minorTickLines: {
    width: 1,
    height: 5,
    color: '#666'
  }
};
```

### Hide Gridlines

```typescript
public primaryYAxis = {
  majorGridLines: { width: 0 },  // Hide major gridlines
  majorTickLines: { width: 0 }   // Hide tick lines
};
```

## Axis Crossing and Inversion

### Axis Crossing

Move axis origin to any value within the chart.

```typescript
public primaryXAxis = {
  crossesAt: 0  // Y-axis crosses X-axis at 0
};

public primaryYAxis = {
  crossesAt: 5  // X-axis crosses Y-axis at 5
};
```

**Use Cases:**
- Zero-centered charts
- Positive/negative data visualization
- Custom axis positioning

### Inversed Axis

Reverse axis direction (high to low).

```typescript
public primaryYAxis = {
  isInversed: true  // Invert Y-axis (top to bottom)
};
```

**Use Cases:**
- Rankings (1st place at top)
- Depth measurements
- Unconventional data presentation

### Opposed Position

Place axis on opposite side.

```typescript
public primaryYAxis = {
  opposedPosition: true  // Place Y-axis on right side
};
```

## Multiple Panes

Split chart area into multiple horizontal sections.

### Row Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  // ...
})
export class AppComponent {
  public rows = [
    { height: '50%' },
    { height: '50%' }
  ];
  
  public axes = [
    { name: 'yAxis1', rowIndex: 0, title: 'Revenue' },
    { name: 'yAxis2', rowIndex: 1, title: 'Profit' }
  ];
}
```

```html
<ejs-chart [rows]="rows" [axes]="axes">
  <e-series-collection>
    <e-series [dataSource]="revenueData" type="Line" xName="x" yName="y" yAxisName="yAxis1"></e-series>
    <e-series [dataSource]="profitData" type="Area" xName="x" yName="y" yAxisName="yAxis2"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Column Configuration

Split vertically:

```typescript
public columns = [
  { width: '50%' },
  { width: '50%' }
];

public axes = [
  { name: 'xAxis1', columnIndex: 0 },
  { name: 'xAxis2', columnIndex: 1 }
];
```

### Grid Layout (Rows + Columns)

```typescript
public rows = [
  { height: '50%' },
  { height: '50%' }
];

public columns = [
  { width: '50%' },
  { width: '50%' }
];

public axes = [
  { name: 'yAxis1', rowIndex: 0, columnIndex: 0 },
  { name: 'yAxis2', rowIndex: 0, columnIndex: 1 },
  { name: 'yAxis3', rowIndex: 1, columnIndex: 0 },
  { name: 'yAxis4', rowIndex: 1, columnIndex: 1 }
];
```

## Chart Title and Subtitle

### Title Configuration

```typescript
public title = 'Monthly Sales Analysis';

public titleStyle = {
  fontFamily: 'Arial',
  size: '20px',
  fontWeight: 'Bold',
  color: '#333',
  textAlignment: 'Center',  // Near, Center, Far
  textOverflow: 'Wrap'  // Wrap, Trim, None
};
```

```html
<ejs-chart [title]="title" [titleStyle]="titleStyle">
  <!-- series -->
</ejs-chart>
```

### Subtitle

```typescript
public subTitle = 'January - December 2024';

public subTitleStyle = {
  size: '14px',
  color: '#666',
  textAlignment: 'Center'
};
```

```html
<ejs-chart [title]="title" [subTitle]="subTitle" 
           [titleStyle]="titleStyle" [subTitleStyle]="subTitleStyle">
  <!-- series -->
</ejs-chart>
```

### Title Position

```typescript
public titleStyle = {
  textAlignment: 'Near'  // Left-aligned title
};
```

## Chart Dimensions

### Fixed Dimensions

```html
<ejs-chart width="800px" height="400px">
  <!-- series -->
</ejs-chart>
```

### Responsive Dimensions

```html
<ejs-chart width="100%" height="450px">
  <!-- series -->
</ejs-chart>
```

### Container-based Sizing

```html
<div style="width: 1200px; height: 600px;">
  <ejs-chart width="100%" height="100%">
    <!-- series -->
  </ejs-chart>
</div>
```

### Aspect Ratio

For responsive designs maintaining proportions:

```css
.chart-container {
  position: relative;
  width: 100%;
  padding-bottom: 50%; /* 2:1 aspect ratio */
}

.chart-container ejs-chart {
  position: absolute;
  width: 100%;
  height: 100%;
}
```

## Margins and Padding

### Chart Margin

Space outside chart area.

```typescript
public margin = {
  left: 40,
  right: 40,
  top: 40,
  bottom: 40
};
```

```html
<ejs-chart [margin]="margin">
  <!-- series -->
</ejs-chart>
```

### Chart Border

```typescript
public border = {
  width: 2,
  color: '#333'
};

public background = 'white';
```

```html
<ejs-chart [border]="border" [background]="background">
  <!-- series -->
</ejs-chart>
```

### Chart Area Border

Border around plot area only (excluding axes, titles).

```typescript
public chartArea = {
  border: {
    width: 2,
    color: '#e0e0e0'
  },
  background: '#f9f9f9'
};
```

```html
<ejs-chart [chartArea]="chartArea">
  <!-- series -->
</ejs-chart>
```

## Best Practices

### Axis Configuration
- Set appropriate `minimum`, `maximum`, and `interval` for clarity
- Use consistent axis types across related charts
- Format labels for readability (K, M, %, $)

### Multiple Axes
- Limit to 2-3 axes maximum to avoid confusion
- Use distinct colors for each axis
- Align scales logically

### Labels
- Rotate when labels overlap (45° or 90°)
- Use multilevel labels for hierarchical data
- Keep font sizes readable (12px minimum)

### Layout
- Use multiple panes for unrelated data series
- Maintain consistent pane sizing
- Add titles to clarify each pane's purpose

### Dimensions
- Set explicit height for proper rendering
- Use percentage width for responsiveness
- Test on various screen sizes

## Common Pitfalls

1. **No Axis Range:** Forgetting `minimum`/`maximum` can result in awkward scales
2. **Overlapping Labels:** Not using `labelIntersectAction` with many categories
3. **Wrong Axis Type:** Using Category axis for continuous numeric data
4. **Too Many Axes:** More than 3 axes creates confusion
5. **Missing Titles:** Unlabeled axes lack context
6. **Fixed Width:** Not responsive on mobile devices

## Troubleshooting

**Labels cut off:**
- Increase chart `margin`
- Use `labelIntersectAction: 'Wrap'` or `'Rotate45'`

**Axis not visible:**
- Check `lineStyle.width` is not 0
- Verify `visible: true` (default)

**Wrong scale:**
- Set explicit `minimum`, `maximum`, `interval`
- Check `valueType` matches data

**Multiple axes not showing:**
- Ensure `name` property is unique
- Bind series using correct `yAxisName`/`xAxisName`

Refer to interactive-features for axis-based interactions like zooming and panning.

## API Reference Summary

### Core Axis APIs

| API | Description | Documentation |
|-----|-------------|---------------|
| AxisModel | Complete axis configuration interface with 50+ properties | [axisModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/axisModel) |
| Axis | Axis class with methods and properties | [axis.md](https://ej2.syncfusion.com/angular/documentation/api/chart/axis) |
| AxisDirective | Axis directive for declaring multiple axes | [axisDirective.md](https://ej2.syncfusion.com/angular/documentation/api/chart/axisDirective) |

### Axis Configuration Properties

| Property | Type | Default | Description | API Reference |
|----------|------|---------|-------------|---------------|
| valueType | ValueType | 'Double' | Axis value type: Double, DateTime, Category, Logarithmic | [Axis.valueType](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#valueType), [ValueType](https://ej2.syncfusion.com/angular/documentation/api/chart/valueType) |
| name | string | '' | Unique identifier for the axis | [Axis.name](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#name) |
| title | string | '' | Axis title text | [Axis.title](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#title) |
| minimum | Object | null | Minimum axis value | [Axis.minimum](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#minimum) |
| maximum | Object | null | Maximum axis value | [Axis.maximum](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#maximum) |
| interval | number | null | Axis label interval | [Axis.interval](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#interval) |
| intervalType | IntervalType | 'Auto' | DateTime interval type | [Axis.intervalType](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#intervalType), [IntervalType](https://ej2.syncfusion.com/angular/documentation/api/chart/intervalType) |
| logBase | number | 10 | Logarithmic base value | [Axis.logBase](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#logBase) |
| rangePadding | ChartRangePadding | 'Auto' | Padding at axis ends | [Axis.rangePadding](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#rangePadding), [ChartRangePadding](https://ej2.syncfusion.com/angular/documentation/api/chart/chartRangePadding) |
| opposedPosition | boolean | false | Place axis on opposite side | [Axis.opposedPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#opposedPosition) |
| isInversed | boolean | false | Invert axis direction | [Axis.isInversed](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#isInversed) |
| crossesAt | Object | null | Axis crossing point | [Axis.crossesAt](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#crossesAt) |
| crossesInAxis | string | null | Cross at specific axis | [Axis.crossesInAxis](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#crossesInAxis) |
| rowIndex | number | 0 | Row index for multiple rows | [Axis.rowIndex](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#rowIndex) |
| columnIndex | number | 0 | Column index for multiple columns | [Axis.columnIndex](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#columnIndex) |
| span | number | 1 | Number of rows/columns to span | [Axis.span](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#span) |

### Label Configuration

| Property | Type | Default | Description | API Reference |
|----------|------|---------|-------------|---------------|
| labelFormat | string | '' | Label format string | [Axis.labelFormat](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelFormat) |
| labelStyle | FontModel | - | Label font styling | [Axis.labelStyle](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelStyle), [FontModel](https://ej2.syncfusion.com/angular/documentation/api/chart/fontModel) |
| labelRotation | number | 0 | Label rotation angle (-90 to 90) | [Axis.labelRotation](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelRotation) |
| labelIntersectAction | LabelIntersectAction | 'Trim' | Action for overlapping labels | [Axis.labelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelIntersectAction), [LabelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/labelIntersectAction) |
| labelPlacement | LabelPlacement | 'BetweenTicks' | Label placement relative to ticks | [Axis.labelPlacement](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelPlacement), [LabelPlacement](https://ej2.syncfusion.com/angular/documentation/api/chart/labelPlacement) |
| labelPosition | AxisPosition | 'Outside' | Label position (Inside/Outside) | [Axis.labelPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelPosition), [AxisPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/axisPosition) |
| labelPadding | number | 5 | Space between labels and axis | [Axis.labelPadding](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelPadding) |
| maximumLabelWidth | number | 34 | Maximum label width in pixels | [Axis.maximumLabelWidth](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#maximumLabelWidth) |
| enableTrim | boolean | false | Trim labels to fit | [Axis.enableTrim](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#enableTrim) |
| enableWrap | boolean | false | Wrap labels to multiple lines | [Axis.enableWrap](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#enableWrap) |
| labelTemplate | string/Function | null | Custom label template | [Axis.labelTemplate](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#labelTemplate) |

### Gridlines and Tick Lines

| Feature | API Reference |
|---------|---------------|
| Major Gridlines | [MajorGridLinesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/majorGridLinesModel), [Axis.majorGridLines](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#majorGridLines) |
| Minor Gridlines | [MinorGridLinesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/minorGridLinesModel), [Axis.minorGridLines](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#minorGridLines) |
| Major Tick Lines | [MajorTickLinesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/majorTickLinesModel), [Axis.majorTickLines](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#majorTickLines) |
| Minor Tick Lines | [MinorTickLinesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/minorTickLinesModel), [Axis.minorTickLines](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#minorTickLines) |
| Axis Line | [AxisLineModel](https://ej2.syncfusion.com/angular/documentation/api/chart/axisLineModel), [Axis.lineStyle](https://ej2.syncfusion.com/angular/documentation/api/chart/axis#lineStyle) |

### Chart Layout

| Property | Type | Description | API Reference |
|----------|------|-------------|---------------|
| title | string | Chart title | [ChartModel.title](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#title) |
| titleStyle | TitleStyleSettingsModel | Title styling | [TitleStyleSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/titleStyleSettingsModel) |
| width | string | Chart width | [ChartModel.width](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#width) |
| height | string | Chart height | [ChartModel.height](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#height) |
| margin | MarginModel | Chart margins | [ChartModel.margin](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#margin), [MarginModel](https://ej2.syncfusion.com/angular/documentation/api/chart/marginModel) |
| chartArea | ChartAreaModel | Plot area configuration | [ChartModel.chartArea](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#chartArea), [ChartAreaModel](https://ej2.syncfusion.com/angular/documentation/api/chart/chartAreaModel) |
| rows | RowModel[] | Row definitions for multiple panes | [ChartModel.rows](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#rows), [RowModel](https://ej2.syncfusion.com/angular/documentation/api/chart/rowModel) |
| columns | ColumnModel[] | Column definitions for multiple panes | [ChartModel.columns](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#columns), [ColumnModel](https://ej2.syncfusion.com/angular/documentation/api/chart/columnModel) |
| isTransposed | boolean | Transpose/invert chart | [ChartModel.isTransposed](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#isTransposed) |

### Enumerations

| Enum | Description | API Reference |
|------|-------------|---------------|
| ValueType | Axis value types (Double, DateTime, Category, Logarithmic) | [valueType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/valueType) |
| IntervalType | DateTime interval types (Years, Months, Days, Hours, etc.) | [intervalType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/intervalType) |
| ChartRangePadding | Axis padding modes (None, Normal, Additional, Round, Auto) | [chartRangePadding.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartRangePadding) |
| LabelIntersectAction | Label overlap handling (None, Hide, Trim, Wrap, etc.) | [labelIntersectAction.md](https://ej2.syncfusion.com/angular/documentation/api/chart/labelIntersectAction) |
| LabelPlacement | Label placement relative to ticks | [labelPlacement.md](https://ej2.syncfusion.com/angular/documentation/api/chart/labelPlacement) |
| AxisPosition | Label/title position (Inside, Outside) | [axisPosition.md](https://ej2.syncfusion.com/angular/documentation/api/chart/axisPosition) |
| SkeletonType | DateTime skeleton types | [skeletonType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/skeletonType) |
| EdgeLabelPlacement | Edge label placement (None, Hide, Shift) | [edgeLabelPlacement.md](https://ej2.syncfusion.com/angular/documentation/api/chart/edgeLabelPlacement) |
