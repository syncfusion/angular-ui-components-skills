# Chart Elements

Chart elements enhance data visualization by adding context, highlighting patterns, and improving readability. This guide covers legends, markers, data labels, annotations, trendlines, technical indicators, striplines, and gradient fills.

## Table of Contents

- [Legend](#legend)
- [Data Markers](#data-markers)
- [Data Labels](#data-labels)
- [Annotations](#annotations)
- [Trendlines](#trendlines)
- [Technical Indicators](#technical-indicators)
- [Striplines](#striplines)
- [Gradient Fills](#gradient-fills)

## Legend

Legends identify series in charts with multiple data sets, displaying names, colors, and symbols.

**API Reference:**
- [LegendSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettingsModel) - Complete legend configuration
- [LegendSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettings) - Legend settings class
- [LegendPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/legendPosition) - Legend position enum
- [LegendShape](https://ej2.syncfusion.com/angular/documentation/api/chart/legendShape) - Available legend icon shapes
- [LegendMode](https://ej2.syncfusion.com/angular/documentation/api/chart/legendMode) - Legend rendering modes (Series, Point, Range, Gradient)

### Basic Legend Configuration

**API Properties:**
- [LegendSettings.visible](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettings#visible) (boolean, default: true)
- [LegendSettings.position](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettings#position) - [LegendPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/legendPosition) enum

```typescript
public legendSettings = {
  visible: true
};
```

```html
<ejs-chart [legendSettings]="legendSettings">
  <e-series-collection>
    <e-series [dataSource]="data1" type="Line" xName="x" yName="y" name="Product A"></e-series>
    <e-series [dataSource]="data2" type="Line" xName="x" yName="y" name="Product B"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Legend Positioning

```typescript
public legendSettings = {
  visible: true,
  position: 'Top',  // Top, Bottom, Left, Right
  alignment: 'Center'  // Near, Center, Far
};
```

**Position Options:**
- `Top`: Above chart area
- `Bottom`: Below chart area (default)
- `Left`: Left side of chart
- `Right`: Right side of chart
- `Custom`: Specify x, y coordinates

**Custom Positioning:**
```typescript
public legendSettings = {
  visible: true,
  position: 'Custom',
  location: { x: 70, y: 20 }  // Percentage values
};
```

### Legend Shape and Style

```typescript
public legendSettings = {
  visible: true,
  shapeHeight: 15,
  shapeWidth: 15,
  shapePadding: 5,
  border: { width: 1, color: '#000' },
  background: 'transparent',
  opacity: 1,
  textStyle: {
    fontFamily: 'Arial',
    size: '14px',
    fontWeight: '400',
    color: '#333'
  }
};
```

**Legend Shapes:**
- Circle (default for line, area, scatter)
- Rectangle (default for column, bar)
- Triangle, Diamond, Pentagon, etc.

### Legend Click Behavior

Toggle series visibility by clicking legend items:

```typescript
public legendSettings = {
  visible: true,
  toggleVisibility: true  // Enable/disable series on click
};
```

### Legend Paging

For charts with many series:

```typescript
public legendSettings = {
  visible: true,
  width: '200px',
  height: '100px',
  enablePages: true  // Add pagination for overflow
};
```

### Custom Legend Template

```typescript
public legendSettings = {
  visible: true
};
```

```html
<ejs-chart [legendSettings]="legendSettings">
  <ng-template #legendTemplate let-data>
    <div style="display: flex; align-items: center;">
      <span [style.background]="data.fill" style="width: 15px; height: 15px; display: inline-block; margin-right: 5px;"></span>
      <span>{{data.text}} - {{data.percentage}}%</span>
    </div>
  </ng-template>
  <e-series-collection>
    <!-- series -->
  </e-series-collection>
</ejs-chart>
```

## Data Markers

Markers highlight individual data points with shapes, making specific values stand out.

**API Reference:**
- [MarkerSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettingsModel) - Complete marker configuration
- [MarkerSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings) - Marker settings class
- [ChartShape](https://ej2.syncfusion.com/angular/documentation/api/chart/chartShape) - Available marker shapes enum

### Basic Marker Configuration

**API Properties:**
- [MarkerSettings.visible](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings#visible) (boolean, default: false)
- [MarkerSettings.width](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings#width) (number, default: 5)
- [MarkerSettings.height](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings#height) (number, default: 5)
- [MarkerSettings.shape](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings#shape) - [ChartShape](https://ej2.syncfusion.com/angular/documentation/api/chart/chartShape) enum
- [MarkerSettings.fill](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings#fill) (string) - Marker fill color
- [MarkerSettings.border](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings#border) - [BorderModel](https://ej2.syncfusion.com/angular/documentation/api/chart/borderModel)

```typescript
public marker = {
  visible: true,
  width: 10,
  height: 10
};
```

```html
<e-series [dataSource]="data" type="Line" xName="x" yName="y" [marker]="marker"></e-series>
```

### Marker Shapes

```typescript
public marker = {
  visible: true,
  width: 12,
  height: 12,
  shape: 'Circle'  // Circle, Rectangle, Triangle, Diamond, Pentagon, etc.
};
```

**Available Shapes:**
- Circle (default)
- Rectangle / Square
- Triangle
- Diamond
- Pentagon
- Cross / Plus
- Image (use imageUrl property)

### Marker Styling

```typescript
public marker = {
  visible: true,
  width: 10,
  height: 10,
  fill: '#FF5733',
  border: { 
    width: 2, 
    color: '#333' 
  },
  opacity: 1
};
```

### Image Markers

```typescript
public marker = {
  visible: true,
  width: 20,
  height: 20,
  shape: 'Image',
  imageUrl: 'assets/marker-icon.png'
};
```

### Marker for Specific Points

Show markers only at specific data points:

```typescript
public data = [
  { x: 'Jan', y: 35, marker: { visible: true } },
  { x: 'Feb', y: 28, marker: { visible: false } },
  { x: 'Mar', y: 34, marker: { visible: true } }
];
```

## Data Labels

Data labels display values directly on or near chart elements for immediate readability.

### Basic Data Labels

```typescript
public marker = { dataLabel: { visible: true } };
```

```html
<e-series [dataSource]="data" type="Column" xName="x" yName="y" [marker]="marker"></e-series>
```

### Label Positioning

```typescript
public marker = {
  dataLabel: {
    visible: true,
    position: 'Top'  // Top, Bottom, Middle, Outer, Auto
  }
};
```

**Position Options by Series Type:**

**Column/Bar:**
- `Top`: Above bars
- `Bottom`: Below bars
- `Middle`: Center of bars
- `Outer`: Outside bar ends

**Line/Scatter:**
- `Top`: Above points
- `Bottom`: Below points
- `Middle`: On points

**Pie/Donut:**
- `Inside`: Within slices
- `Outside`: Beyond slices

### Label Formatting

```typescript
public marker = {
  dataLabel: {
    visible: true,
    format: '${value}K',  // Format string
    font: {
      fontFamily: 'Arial',
      size: '12px',
      fontWeight: 'Bold',
      color: '#333'
    }
  }
};
```

**Format Patterns:**
- `${value}`: Raw value
- `${point.x}`: X-axis value
- `${point.y}`: Y-axis value
- `${series.name}`: Series name
- `${point.percentage}%`: Percentage (for pie charts)

### Label Border and Background

```typescript
public marker = {
  dataLabel: {
    visible: true,
    border: { 
      width: 2, 
      color: '#FF5733' 
    },
    fill: 'white',
    margin: { left: 5, right: 5, top: 5, bottom: 5 }
  }
};
```

### Smart Labels

Avoid label overlapping:

```typescript
public marker = {
  dataLabel: {
    visible: true,
    enableSmartLabels: true,  // Adjust labels to prevent overlap
    labelIntersectAction: 'Hide'  // Hide, Rotate45, Rotate90, Trim, Wrap
  }
};
```

### Custom Label Template

```html
<e-series [dataSource]="data" type="Column" xName="x" yName="y"[marker]="marker"></e-series>
```

```typescript
public marker = {
  dataLabel: {
    visible: true,
    template: `
      <div style="background: \${point.color}; padding: 3px; border-radius: 3px; color: white;">
        <b>\${point.x}</b>: $\${point.y}K
      </div>
    `
  }
};
```

## Annotations

Annotations add custom HTML elements, text, shapes, or images at specific chart coordinates.

### Text Annotation

```typescript
import { AnnotationService } from '@syncfusion/ej2-angular-charts';

@Component({
  // ...
  providers: [AnnotationService]
})
export class AppComponent {
  public annotations = [{
    content: '<div style="background: #FF5733; color: white; padding: 5px;">Peak Sales</div>',
    x: 'May',
    y: 40,
    coordinateUnits: 'Point',  // Pixel or Point
    region: 'Series'  // Chart or Series
  }];
}
```

```html
<ejs-chart [annotations]="annotations">
  <e-series-collection>
    <e-series [dataSource]="data" type="Line" xName="x" yName="y"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Coordinate Units

```typescript
public annotations = [{
  content: '<div>Annotation Text</div>',
  x: 50,  // X position
  y: 50,  // Y position
  coordinateUnits: 'Pixel',  // Pixel: absolute position, Point: data coordinates
  verticalAlignment: 'Top',  // Top, Middle, Bottom
  horizontalAlignment: 'Center',  // Near, Center, Far
  region: 'Chart'  // Chart: entire chart area, Series: plot area
}];
```

### Image Annotation

```typescript
public annotations = [{
  content: '<img src="assets/warning-icon.png" width="30" height="30"/>',
  x: 'Mar',
  y: 34,
  coordinateUnits: 'Point',
  region: 'Series'
}];
```

### Shape Annotation

```typescript
public annotations = [{
  content: '<div style="width: 100px; height: 50px; background: rgba(255,0,0,0.2); border: 2px solid red;"></div>',
  x: 'Feb',
  y: 28,
  coordinateUnits: 'Point',
  region: 'Series'
}];
```

### Multiple Annotations

```typescript
public annotations = [
  {
    content: '<div>Annotation 1</div>',
    x: 'Jan',
    y: 35,
    coordinateUnits: 'Point'
  },
  {
    content: '<div>Annotation 2</div>',
    x: 'May',
    y: 40,
    coordinateUnits: 'Point'
  }
];
```

## Trendlines

Trendlines show overall patterns or directional trends in data.

### Linear Trendline

```typescript
import { TrendlinesService } from '@syncfusion/ej2-angular-charts';

@Component({
  // ...
  providers: [TrendlinesService]
})
export class AppComponent {
  public trendlines = [{
    type: 'Linear',
    width: 2,
    name: 'Linear Trend',
    fill: '#FF5733',
    enableTooltip: true
  }];
}
```

```html
<e-series [dataSource]="data" type="Scatter" xName="x" yName="y" [trendlines]="trendlines"></e-series>
```

### Trendline Types

```typescript
public trendlines = [{
  type: 'Linear'  // Linear, Exponential, Logarithmic, Polynomial, Power, MovingAverage
}];
```

**Type Descriptions:**
- **Linear:** Straight line fit
- **Exponential:** Exponential curve
- **Logarithmic:** Logarithmic curve
- **Polynomial:** Polynomial fit (specify `polynomialOrder`)
- **Power:** Power function
- **MovingAverage:** Smoothed average (specify `period`)

### Polynomial Trendline

```typescript
public trendlines = [{
  type: 'Polynomial',
  polynomialOrder: 3,  // Order of polynomial
  width: 2,
  fill: '#3498db'
}];
```

### Moving Average

```typescript
public trendlines = [{
  type: 'MovingAverage',
  period: 3,  // Number of points to average
  width: 2,
  fill: '#9b59b6'
}];
```

### Forward/Backward Forecast

```typescript
public trendlines = [{
  type: 'Linear',
  forwardForecast: 5,  // Extend forward
  backwardForecast: 2,  // Extend backward
  width: 2,
  dashArray: '5,5'  // Dashed line for forecast
}];
```

### Trendline Styling

```typescript
public trendlines = [{
  type: 'Linear',
  width: 3,
  fill: '#e74c3c',
  dashArray: '10,5',  // Dash pattern
  opacity: 0.7,
  marker: {
    visible: true,
    width: 8,
    height: 8
  },
  intercept: 10  // Force specific intercept
}];
```

## Technical Indicators

Technical indicators for financial chart analysis (RSI, MACD, Bollinger Bands, etc.).

### RSI (Relative Strength Index)

```typescript
import { TechnicalIndicatorService } from '@syncfusion/ej2-angular-charts';

@Component({
  providers: [TechnicalIndicatorService]
})
export class AppComponent {
  public indicators = [{
    type: 'Rsi',
    field: 'Close',
    seriesName: 'Stock',
    yAxisName: 'secondary',
    period: 14,
    fill: '#6063ff',
    upperBand: 70,
    lowerBand: 30
  }];
}
```

### MACD (Moving Average Convergence Divergence)

```typescript
public indicators = [{
  type: 'Macd',
  field: 'Close',
  seriesName: 'Stock',
  yAxisName: 'secondary',
  fastPeriod: 12,
  slowPeriod: 26,
  trigger: 9,
  macdType: 'Both',  // Both, Line, Histogram
  fill: '#6063ff'
}];
```

### Bollinger Bands

```typescript
public indicators = [{
  type: 'BollingerBands',
  field: 'Close',
  seriesName: 'Stock',
  period: 14,
  standardDeviation: 2,
  fill: '#6063ff',
  upperLine: { color: '#e74c3c', width: 1 },
  lowerLine: { color: '#3498db', width: 1 }
}];
```

### Other Indicators

**EMA (Exponential Moving Average):**
```typescript
public indicators = [{ type: 'Ema', field: 'Close', period: 14 }];
```

**SMA (Simple Moving Average):**
```typescript
public indicators = [{ type: 'Sma', field: 'Close', period: 14 }];
```

**ATR (Average True Range):**
```typescript
public indicators = [{ type: 'Atr', field: 'Close', period: 14 }];
```

**Stochastic:**
```typescript
public indicators = [{
  type: 'Stochastic',
  period: 14,
  kPeriod: 3,
  dPeriod: 3,
  field: 'Close'
}];
```

## Striplines

Striplines highlight specific axis ranges with colored bands and labels.

### Horizontal Stripline

```typescript
public primaryYAxis = {
  stripLines: [{
    start: 30,
    end: 40,
    text: 'Target Range',
    color: 'rgba(255, 0, 0, 0.1)',
    border: { width: 1, color: 'red' },
    textStyle: { 
      color: 'red', 
      size: '14px' 
    },
    horizontalAlignment: 'Start',
    verticalAlignment: 'Start',
    visible: true
  }]
};
```

### Vertical Stripline

```typescript
public primaryXAxis = {
  valueType: 'Category',
  stripLines: [{
    start: 2,  // Index or value
    end: 4,
    text: 'Q2',
    color: 'rgba(0, 128, 255, 0.1)',
    border: { width: 2, color: 'blue' },
    textStyle: { color: 'blue' }
  }]
};
```

### Recurring Striplines

```typescript
public primaryXAxis = {
  stripLines: [{
    startFromAxis: true,
    size: 1,
    sizeType: 'Auto',
    isRepeat: true,
    repeatEvery: 2,
    color: 'rgba(0, 0, 0, 0.05)'
  }]
};
```

### Line Stripline (Single Line)

```typescript
public stripLines = [{
  start: 35,
  size: 0,  // Zero size = single line
  color: 'red',
  border: { width: 2, color: 'red' },
  text: 'Average',
  textStyle: { color: 'red' }
}];
```

## Gradient Fills

Apply gradient colors to series for visual depth.

### Linear Gradient

```typescript
public fill = 'url(#gradient1)';
```

```html
<ejs-chart>
  <svg style="height: 0">
    <defs>
      <linearGradient id="gradient1" x1="0%" y1="0%" x2="0%" y2="100%">
        <stop offset="0%" style="stop-color:#3498db;stop-opacity:1" />
        <stop offset="100%" style="stop-color:#e74c3c;stop-opacity:1" />
      </linearGradient>
    </defs>
  </svg>
  <e-series-collection>
    <e-series [dataSource]="data" type="Column" xName="x" yName="y" [fill]="fill"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Radial Gradient

```html
<radialGradient id="gradient2">
  <stop offset="0%" style="stop-color:#fff;stop-opacity:1" />
  <stop offset="100%" style="stop-color:#3498db;stop-opacity:1" />
</radialGradient>
```

### Multiple Series with Gradients

```typescript
public fill1 = 'url(#gradient1)';
public fill2 = 'url(#gradient2)';
```

```html
<e-series [dataSource]="data1" type="Column" xName="x" yName="y" [fill]="fill1"></e-series>
<e-series [dataSource]="data2" type="Column" xName="x" yName="y" [fill]="fill2"></e-series>
```

## Best Practices

### Legend
- Position legends where they don't obscure data
- Use descriptive series names
- Enable toggle visibility for interactive exploration

### Markers
- Use sparingly on line charts (only key points)
- Consistent shapes across related series
- Larger markers for emphasis

### Data Labels
- Enable only when space permits
- Use formatting for readability ($, %, K, M)
- Smart labels to avoid overlap

### Annotations
- Highlight key insights or anomalies
- Keep text concise
- Use contrasting colors

### Trendlines
- Choose appropriate type for data pattern
- Use dashed lines for forecasts
- Match colors with series

### Striplines
- Highlight thresholds or goals
- Use subtle colors (low opacity)
- Label clearly

## Common Pitfalls

1. **Too Many Elements:** Cluttered charts with excessive markers, labels, annotations
2. **Poor Contrast:** Annotations/labels hard to read against chart background
3. **Overlapping Labels:** Not using smart labels or label rotation
4. **Inconsistent Styling:** Mixed marker shapes/sizes without purpose
5. **Missing Context:** Annotations without explanatory text

Refer to customization reference for detailed styling options.

## API Reference Summary

### Legend Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| LegendSettingsModel | Complete legend configuration interface | [legendSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettingsModel) |
| LegendSettings | Legend settings class | [legendSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettings) |
| LegendPosition | Position enum (Top, Bottom, Left, Right, Custom) | [legendPosition.md](https://ej2.syncfusion.com/angular/documentation/api/chart/legendPosition) |
| LegendShape | Icon shape enum (Circle, Rectangle, Triangle, etc.) | [legendShape.md](https://ej2.syncfusion.com/angular/documentation/api/chart/legendShape) |
| LegendMode | Rendering mode (Series, Point, Range, Gradient) | [legendMode.md](https://ej2.syncfusion.com/angular/documentation/api/chart/legendMode) |

### Marker Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| MarkerSettingsModel | Complete marker configuration interface | [markerSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettingsModel) |
| MarkerSettings | Marker settings class | [markerSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings) |
| ChartShape | Marker shape enum (Circle, Rectangle, Diamond, etc.) | [chartShape.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartShape) |

### Data Label Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| DataLabelSettingsModel | Data label configuration interface | [dataLabelSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelSettingsModel) |
| DataLabelSettings | Data label settings class | [dataLabelSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelSettings) |
| DataLabelIntersectAction | Overlap handling (None, Hide, Rotate, etc.) | [dataLabelIntersectAction.md](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelIntersectAction) |
| LabelPosition | Label position enum | [labelPosition.md](https://ej2.syncfusion.com/angular/documentation/api/chart/labelPosition) |
| Alignment | Text alignment enum | [alignment.md](https://ej2.syncfusion.com/angular/documentation/api/chart/alignment) |

### Annotation Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| ChartAnnotationSettingsModel | Annotation configuration interface | [chartAnnotationSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartAnnotationSettingsModel) |
| ChartAnnotationSettings | Annotation settings class | [chartAnnotationSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartAnnotationSettings) |
| AnnotationDirective | Annotation directive for declaring annotations | [annotationDirective.md](https://ej2.syncfusion.com/angular/documentation/api/chart/annotationDirective) |
| Anchor | Anchor point enum (Chart, Series, Point) | [anchor.md](https://ej2.syncfusion.com/angular/documentation/api/chart/anchor) |
| Position | Positioning enum | [position.md](https://ej2.syncfusion.com/angular/documentation/api/chart/position) |

### Trendline Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| TrendlineModel | Trendline configuration interface | [trendlineModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/trendlineModel) |
| Trendline | Trendline class | [trendline.md](https://ej2.syncfusion.com/angular/documentation/api/chart/trendline) |
| TrendlineDirective | Trendline directive | [trendlineDirective.md](https://ej2.syncfusion.com/angular/documentation/api/chart/trendlineDirective) |
| TrendlineTypes | Types enum (Linear, Exponential, Polynomial, etc.) | [trendlineTypes.md](https://ej2.syncfusion.com/angular/documentation/api/chart/trendlineTypes) |

### Technical Indicator Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| TechnicalIndicatorModel | Indicator configuration interface | [technicalIndicatorModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/technicalIndicatorModel) |
| TechnicalIndicator | Indicator class | [technicalIndicator.md](https://ej2.syncfusion.com/angular/documentation/api/chart/technicalIndicator) |
| IndicatorDirective | Indicator directive | [indicatorDirective.md](https://ej2.syncfusion.com/angular/documentation/api/chart/indicatorDirective) |
| TechnicalIndicators | Indicator types enum (SMA, EMA, RSI, MACD, etc.) | [technicalIndicators.md](https://ej2.syncfusion.com/angular/documentation/api/chart/technicalIndicators) |
| MacdType | MACD type enum | [macdType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/macdType) |

### Stripline Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| StripLineSettingsModel | Stripline configuration interface | [stripLineSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/stripLineSettingsModel) |
| StripLineSettings | Stripline settings class | [stripLineSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/stripLineSettings) |
| StripLineDirective | Stripline directive | [stripLineDirective.md](https://ej2.syncfusion.com/angular/documentation/api/chart/stripLineDirective) |

### Key Properties Reference

| Element | Important Properties | API Reference |
|---------|---------------------|---------------|
| **Legend** | visible, position, alignment, toggleVisibility | [LegendSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettingsModel) |
| **Marker** | visible, width, height, shape, fill, border | [MarkerSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettingsModel) |
| **Data Label** | visible, position, format, fill, border, angle | [DataLabelSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelSettingsModel) |
| **Annotation** | content, coordinateUnits, x, y, region | [ChartAnnotationSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/chartAnnotationSettingsModel) |
| **Trendline** | type, forward, backward, polynomial Order | [TrendlineModel](https://ej2.syncfusion.com/angular/documentation/api/chart/trendlineModel) |
| **Indicator** | type, period, field, seriesName | [TechnicalIndicatorModel](https://ej2.syncfusion.com/angular/documentation/api/chart/technicalIndicatorModel) |
| **Stripline** | start, end, size, text, color, opacity | [StripLineSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/stripLineSettingsModel) |

### Events Related to Chart Elements

| Event | Interface | Description | API Reference |
|-------|-----------|-------------|---------------|
| legendRender | ILegendRenderEventArgs | Before legend rendering | [ChartModel.legendRender](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#legendRender), [ILegendRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iLegendRenderEventArgs) |
| legendClick | ILegendClickEventArgs | Legend item click | [ChartModel.legendClick](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#legendClick), [ILegendClickEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iLegendClickEventArgs) |
| pointRender | IPointRenderEventArgs | Before point rendering (markers) | [ChartModel.pointRender](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#pointRender), [IPointRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPointRenderEventArgs) |
| textRender | ITextRenderEventArgs | Before text rendering (data labels) | [ChartModel.textRender](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#textRender), [ITextRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iTextRenderEventArgs) |
| annotationRender | IAnnotationRenderEventArgs | Before annotation rendering | [ChartModel.annotationRender](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#annotationRender), [IAnnotationRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iAnnotationRenderEventArgs) |
