# Customization Reference for Syncfusion Angular Chart

## Table of Contents

- [Introduction](#introduction)
- [Color Palettes](#color-palettes)
  - [Built-in Palettes](#built-in-palettes)
  - [Custom Color Palettes](#custom-color-palettes)
  - [Per-Series Colors](#per-series-colors)
- [Themes](#themes)
  - [Available Themes](#available-themes)
  - [Applying Themes](#applying-themes)
  - [Theme Studio](#theme-studio)
- [Chart Area Customization](#chart-area-customization)
  - [Background and Border](#background-and-border)
  - [Chart Margin](#chart-margin)
  - [Plot Area Customization](#plot-area-customization)
- [Series Customization](#series-customization)
  - [Series Appearance](#series-appearance)
  - [Point-Level Customization](#point-level-customization)
  - [Marker Customization](#marker-customization)
- [Text and Label Customization](#text-and-label-customization)
  - [Data Labels](#data-labels)
  - [Axis Labels](#axis-labels)
  - [Title and Subtitle](#title-and-subtitle)
- [Animation Settings](#animation-settings)
- [Responsive Design](#responsive-design)
- [CSS Customization](#css-customization)
- [Dynamic Styling](#dynamic-styling)
- [Best Practices](#best-practices)
- [Advanced Customization](#advanced-customization)
- [Troubleshooting](#troubleshooting)

## Introduction

The Syncfusion Angular Chart component offers extensive customization options to match your application's design language. From built-in themes to granular styling at the point level, you can create visually appealing and consistent data visualizations.

This reference covers all aspects of chart customization including themes, colors, styling, animations, and responsive design.

## Color Palettes

### Built-in Palettes

The chart provides default color palettes that automatically cycle through colors for multiple series.

**Default Palette:**
```typescript
['#00bdae', '#404041', '#357cd2', '#e56590', '#f8b883', 
 '#70ad47', '#dd8abd', '#7f84e8', '#7bb4eb', '#ea7a57']
```

### Custom Color Palettes

Define custom color schemes using the `palettes` property:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-chart',
  template: `
    <ejs-chart [palettes]='customPalette'>
      <e-series-collection>
        <e-series [dataSource]='data1' type='Column'></e-series>
        <e-series [dataSource]='data2' type='Column'></e-series>
        <e-series [dataSource]='data3' type='Column'></e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class ChartComponent {
  public customPalette: string[] = [
    '#FF6B6B',  // Red
    '#4ECDC4',  // Teal
    '#45B7D1',  // Blue
    '#FFA07A',  // Salmon
    '#98D8C8'   // Mint
  ];
}
```

**Brand Colors Example:**
```typescript
public brandPalette: string[] = [
  '#1976D2',  // Primary
  '#FF9800',  // Accent
  '#4CAF50',  // Success
  '#F44336',  // Error
  '#9C27B0'   // Secondary
];
```

**Monochrome Palette:**
```typescript
public monochromePalette: string[] = [
  '#000000',
  '#404040',
  '#808080',
  '#BFBFBF',
  '#E0E0E0'
];
```

### Per-Series Colors

Override palette colors for individual series:

```typescript
<e-series 
  [dataSource]='salesData' 
  type='Column'
  fill='#FF5733'
  name='Sales'>
</e-series>

<e-series 
  [dataSource]='revenueData' 
  type='Line'
  fill='#33C4FF'
  width='3'
  name='Revenue'>
</e-series>
```

## Themes

### Available Themes

Syncfusion provides multiple built-in themes:

| Theme | Description | CSS File |
|-------|-------------|----------|
| Material | Google Material Design | material.css |
| Material Dark | Material Design (Dark) | material-dark.css |
| Fabric | Microsoft Fabric | fabric.css |
| Fabric Dark | Fabric (Dark) | fabric-dark.css |
| Bootstrap | Bootstrap 4 | bootstrap.css |
| Bootstrap Dark | Bootstrap 4 (Dark) | bootstrap-dark.css |
| Bootstrap 5 | Bootstrap 5 | bootstrap5.css |
| Bootstrap 5 Dark | Bootstrap 5 (Dark) | bootstrap5-dark.css |
| Tailwind | Tailwind CSS | tailwind.css |
| Tailwind Dark | Tailwind CSS (Dark) | tailwind-dark.css |
| Fluent | Microsoft Fluent | fluent.css |
| Fluent Dark | Fluent (Dark) | fluent-dark.css |
| High Contrast | Accessibility | highcontrast.css |

### Applying Themes

**1. Import Theme in angular.json:**
```json
{
  "styles": [
    "node_modules/@syncfusion/ej2-angular-charts/styles/material.css"
  ]
}
```

**2. Import in styles.css:**
```css
@import '@syncfusion/ej2-base/styles/material.css';
@import '@syncfusion/ej2-buttons/styles/material.css';
@import '@syncfusion/ej2-popups/styles/material.css';
@import '@syncfusion/ej2-angular-charts/styles/material.css';
```

**3. Dynamic Theme Switching:**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-chart',
  template: `
    <select (change)="changeTheme($event.target.value)">
      <option value="material">Material</option>
      <option value="fabric">Fabric</option>
      <option value="bootstrap">Bootstrap</option>
    </select>
    <ejs-chart [theme]='currentTheme'>
      <!-- chart configuration -->
    </ejs-chart>
  `
})
export class ChartComponent {
  public currentTheme: string = 'Material';
  
  changeTheme(theme: string): void {
    this.currentTheme = theme;
    this.loadThemeCSS(theme);
  }
  
  loadThemeCSS(theme: string): void {
    let link = document.getElementById('theme-link') as HTMLLinkElement;
    if (link) {
      link.href = `node_modules/@syncfusion/ej2-angular-charts/styles/${theme}.css`;
    }
  }
}
```

### Theme Studio

Use Syncfusion Theme Studio to create custom themes:

1. Visit: https://ej2.syncfusion.com/themestudio/
2. Customize colors, fonts, and component styles
3. Download the generated theme CSS
4. Import in your application

**Custom Theme Example:**
```css
/* custom-theme.css */
.e-chart {
  --chart-bg: #F5F5F5;
  --chart-title: #333333;
  --chart-axis-label: #666666;
  --chart-grid: #E0E0E0;
}
```

## Chart Area Customization

### Background and Border

Customize the entire chart background and border:

```typescript
@Component({
  template: `
    <ejs-chart 
      [background]='chartBackground'
      [border]='chartBorder'>
      <e-series-collection>
        <e-series [dataSource]='data'></e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class ChartComponent {
  public chartBackground: string = '#F0F8FF';
  
  public chartBorder: Object = {
    width: 2,
    color: '#4682B4'
  };
}
```

**Gradient Background:**
```typescript
ngAfterViewInit() {
  let chart = document.querySelector('.e-chart');
  if (chart) {
    chart.style.background = 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)';
  }
}
```

### Chart Margin

Control spacing around the chart:

```typescript
public margin: Object = {
  left: 40,
  right: 40,
  top: 40,
  bottom: 40
};

<ejs-chart [margin]='margin'>
```

**Responsive Margins:**
```typescript
@HostListener('window:resize')
onResize() {
  if (window.innerWidth < 768) {
    this.margin = { left: 10, right: 10, top: 20, bottom: 20 };
  } else {
    this.margin = { left: 40, right: 40, top: 40, bottom: 40 };
  }
}
```

### Plot Area Customization

Style the area where data is plotted:

```typescript
public chartArea: Object = {
  border: {
    color: '#CCCCCC',
    width: 1
  },
  background: '#FFFFFF',
  opacity: 0.9
};

<ejs-chart [chartArea]='chartArea'>
```

**With Custom Dimensions:**
```typescript
public chartArea: Object = {
  width: '90%',
  height: '80%',
  background: 'rgba(173, 216, 230, 0.3)',
  border: { width: 0 }
};
```

## Series Customization

### Series Appearance

Customize individual series appearance:

```typescript
<e-series 
  [dataSource]='data'
  type='Column'
  fill='#FF6347'
  opacity='0.8'
  [border]='seriesBorder'
  [cornerRadius]='cornerRadius'>
</e-series>

public seriesBorder: Object = {
  width: 2,
  color: '#8B0000'
};

public cornerRadius: Object = {
  bottomLeft: 10,
  bottomRight: 10,
  topLeft: 10,
  topRight: 10
};
```

**Line Series Customization:**
```typescript
<e-series 
  [dataSource]='data'
  type='Line'
  fill='#4169E1'
  width='3'
  dashArray='5,5'>  <!-- Dashed line -->
</e-series>
```

**Area Series with Gradient:**
```typescript
<e-series 
  [dataSource]='data'
  type='Area'
  fill='url(#gradient1)'
  opacity='0.6'>
</e-series>

// Define gradient in component
ngAfterViewInit() {
  let svg = document.querySelector('svg');
  let defs = document.createElementNS('http://www.w3.org/2000/svg', 'defs');
  defs.innerHTML = `
    <linearGradient id="gradient1" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%" style="stop-color:#4169E1;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#4169E1;stop-opacity:0.3" />
    </linearGradient>
  `;
  svg.insertBefore(defs, svg.firstChild);
}
```

### Point-Level Customization

Use `pointRender` event for dynamic point styling:

```typescript
@Component({
  template: `
    <ejs-chart (pointRender)='pointRender($event)'>
      <e-series-collection>
        <e-series [dataSource]='data' xName='x' yName='y'></e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class ChartComponent {
  pointRender(args: IPointRenderEventArgs): void {
    // Conditional coloring based on value
    if (args.point.y > 50) {
      args.fill = '#00C853';  // Green for high values
    } else if (args.point.y < 20) {
      args.fill = '#FF1744';  // Red for low values
    } else {
      args.fill = '#FFC107';  // Yellow for medium values
    }
    
    // Custom border
    args.border = {
      width: 2,
      color: '#000000'
    };
  }
}
```

**Highlight Specific Points:**
```typescript
pointRender(args: IPointRenderEventArgs): void {
  // Highlight maximum value
  if (args.point.y === Math.max(...this.data.map(d => d.y))) {
    args.fill = '#FF4081';
    args.border = { width: 3, color: '#C51162' };
  }
  
  // Different shapes for ranges
  if (args.point.y > 75) {
    args.shape = 'Diamond';
  }
}
```

### Marker Customization

Enhance data points with custom markers:

```typescript
<e-series 
  [dataSource]='data'
  type='Line'
  [marker]='markerSettings'>
</e-series>

public markerSettings: Object = {
  visible: true,
  shape: 'Circle',  // Circle, Rectangle, Triangle, Diamond, Pentagon, etc.
  width: 10,
  height: 10,
  fill: '#FF6347',
  border: {
    width: 2,
    color: '#FFFFFF'
  },
  imageUrl: 'path/to/custom-marker.png'  // Use custom image
};
```

**Dynamic Marker Shapes:**
```typescript
public markerSettings: Object = {
  visible: true,
  width: 8,
  height: 8,
  dataLabel: {
    visible: true
  }
};

pointRender(args: IPointRenderEventArgs): void {
  // Different shapes based on data
  if (args.point.y > 60) {
    args.shape = 'Triangle';
  } else if (args.point.y > 30) {
    args.shape = 'Circle';
  } else {
    args.shape = 'InvertedTriangle';
  }
}
```

## Text and Label Customization

### Data Labels

Customize labels displayed on data points:

```typescript
<e-series 
  [dataSource]='data'
  [marker]='markerSettings'>
</e-series>

public markerSettings: Object = {
  dataLabel: {
    visible: true,
    position: 'Top',  // Top, Bottom, Middle, Outer
    font: {
      fontFamily: 'Arial',
      size: '12px',
      fontWeight: '600',
      color: '#333333'
    },
    fill: '#FFFFFF',
    border: {
      width: 1,
      color: '#CCCCCC'
    },
    margin: {
      left: 5,
      right: 5,
      top: 5,
      bottom: 5
    },
    rx: 5,  // Border radius x
    ry: 5   // Border radius y
  }
};
```

**Formatted Data Labels:**
```typescript
public markerSettings: Object = {
  dataLabel: {
    visible: true,
    template: '<div style="padding:5px; background:#4CAF50; color:white; border-radius:3px;">${point.y}K</div>'
  }
};
```

**Custom Label Text:**
```typescript
textRender(args: ITextRenderEventArgs): void {
  // Add currency symbol
  args.text = '$' + args.text;
  
  // Format numbers
  args.text = parseFloat(args.text).toFixed(2) + '%';
  
  // Conditional formatting
  if (parseFloat(args.text) < 0) {
    args.color = '#FF0000';
    args.text = '(' + Math.abs(parseFloat(args.text)) + ')';
  }
}
```

### Axis Labels

Style axis labels:

```typescript
public primaryXAxis: Object = {
  labelStyle: {
    color: '#424242',
    size: '12px',
    fontFamily: 'Segoe UI',
    fontWeight: '500'
  },
  labelRotation: -45,  // Rotate labels
  labelIntersectAction: 'Rotate45'  // Handle overlapping
};

public primaryYAxis: Object = {
  labelStyle: {
    color: '#424242',
    size: '12px'
  },
  labelFormat: '{value}%',  // Format as percentage
  edgeLabelPlacement: 'Shift'
};
```

**Custom Axis Label Rendering:**
```typescript
axisLabelRender(args: IAxisLabelRenderEventArgs): void {
  // Abbreviate large numbers
  if (args.value >= 1000000) {
    args.text = (args.value / 1000000).toFixed(1) + 'M';
  } else if (args.value >= 1000) {
    args.text = (args.value / 1000).toFixed(1) + 'K';
  }
  
  // Color-code labels
  if (args.axis.name === 'primaryYAxis' && args.value < 0) {
    args.labelStyle.color = '#FF0000';
  }
}
```

### Title and Subtitle

Customize chart title and subtitle:

```typescript
public title: string = 'Sales Performance 2024';
public titleStyle: Object = {
  fontFamily: 'Arial',
  size: '18px',
  fontWeight: 'bold',
  color: '#2C3E50',
  textAlignment: 'Center',
  textOverflow: 'Wrap'
};

public subTitle: string = 'Quarterly Revenue Analysis';
public subTitleStyle: Object = {
  fontFamily: 'Arial',
  size: '14px',
  color: '#7F8C8D',
  textAlignment: 'Center'
};

<ejs-chart 
  [title]='title'
  [titleStyle]='titleStyle'
  [subTitle]='subTitle'
  [subTitleStyle]='subTitleStyle'>
</ejs-chart>
```

## Animation Settings

Control chart animations:

```typescript
public animation: Object = {
  enable: true,
  duration: 1500,  // milliseconds
  delay: 100       // delay before animation starts
};

<e-series 
  [dataSource]='data'
  [animation]='animation'>
</e-series>
```

**Different Animations Per Series:**
```typescript
public series1Animation: Object = {
  enable: true,
  duration: 1000,
  delay: 0
};

public series2Animation: Object = {
  enable: true,
  duration: 1000,
  delay: 500  // Start after first series
};

<e-series [animation]='series1Animation'></e-series>
<e-series [animation]='series2Animation'></e-series>
```

**Disable Animation for Performance:**
```typescript
// For charts with many data points
public animation: Object = {
  enable: false
};
```

## Responsive Design

Make charts responsive to different screen sizes:

```typescript
import { Component, HostListener } from '@angular/core';

@Component({
  selector: 'app-chart',
  template: `
    <ejs-chart 
      [width]='chartWidth'
      [height]='chartHeight'
      [margin]='margin'>
      <!-- chart configuration -->
    </ejs-chart>
  `
})
export class ResponsiveChartComponent {
  public chartWidth: string = '100%';
  public chartHeight: string = '400px';
  public margin: Object = { left: 40, right: 40, top: 40, bottom: 40 };
  
  @HostListener('window:resize')
  onResize() {
    const width = window.innerWidth;
    
    if (width < 576) {
      // Mobile
      this.chartHeight = '300px';
      this.margin = { left: 10, right: 10, top: 20, bottom: 40 };
      this.primaryXAxis.labelRotation = -45;
    } else if (width < 768) {
      // Tablet
      this.chartHeight = '350px';
      this.margin = { left: 20, right: 20, top: 30, bottom: 40 };
      this.primaryXAxis.labelRotation = 0;
    } else {
      // Desktop
      this.chartHeight = '400px';
      this.margin = { left: 40, right: 40, top: 40, bottom: 40 };
      this.primaryXAxis.labelRotation = 0;
    }
  }
  
  ngOnInit() {
    this.onResize();  // Set initial size
  }
}
```

**CSS Media Queries:**
```css
/* chart.component.css */
.chart-container {
  width: 100%;
  padding: 20px;
}

@media (max-width: 768px) {
  .chart-container {
    padding: 10px;
  }
  
  .e-chart .e-chart-title {
    font-size: 14px !important;
  }
  
  .e-chart .e-axis-label {
    font-size: 10px !important;
  }
}

@media (max-width: 576px) {
  .chart-container {
    padding: 5px;
  }
  
  .e-chart .e-chart-title {
    font-size: 12px !important;
  }
}
```

## CSS Customization

### Global Chart Styles

Override default styles:

```css
/* styles.css or component.css */

/* Chart background */
.e-chart {
  background: linear-gradient(to bottom, #f5f7fa 0%, #c3cfe2 100%);
  font-family: 'Roboto', sans-serif;
}

/* Title styling */
.e-chart .e-chart-title {
  font-size: 20px;
  font-weight: 700;
  fill: #2c3e50;
}

/* Axis lines */
.e-chart .e-axis-line {
  stroke: #95a5a6;
  stroke-width: 1;
}

/* Grid lines */
.e-chart .e-grid-line {
  stroke: #ecf0f1;
  stroke-dasharray: 3, 3;
}

/* Legend */
.e-chart .e-legend-text {
  font-size: 12px;
  fill: #34495e;
}

/* Tooltip */
.e-chart .e-tooltip-wrap {
  background: rgba(0, 0, 0, 0.8);
  border-radius: 4px;
  padding: 8px;
}
```

### Component-Level Styling

```typescript
@Component({
  selector: 'app-chart',
  template: `<ejs-chart class="custom-chart"></ejs-chart>`,
  styles: [`
    .custom-chart {
      border: 2px solid #3498db;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    }
  `]
})
```

## Dynamic Styling

Change styles programmatically:

```typescript
export class DynamicChartComponent {
  @ViewChild('chart') chart: ChartComponent;
  
  applyDarkMode(): void {
    this.chart.background = '#2C3E50';
    this.chart.primaryXAxis.labelStyle.color = '#ECF0F1';
    this.chart.primaryYAxis.labelStyle.color = '#ECF0F1';
    this.chart.titleStyle.color = '#FFFFFF';
    this.chart.refresh();
  }
  
  applyLightMode(): void {
    this.chart.background = '#FFFFFF';
    this.chart.primaryXAxis.labelStyle.color = '#2C3E50';
    this.chart.primaryYAxis.labelStyle.color = '#2C3E50';
    this.chart.titleStyle.color = '#2C3E50';
    this.chart.refresh();
  }
}
```

## Best Practices

1. **Maintain Consistency:**
   - Use consistent colors across related charts
   - Follow your brand guidelines
   - Keep font styles uniform

2. **Accessibility:**
   - Ensure sufficient color contrast (WCAG AA: 4.5:1 minimum)
   - Don't rely solely on color to convey information
   - Provide alternative text for screen readers

3. **Performance:**
   - Disable animations for charts with many points
   - Use CSS for static styles instead of inline styles
   - Minimize DOM manipulation

4. **Responsive Design:**
   - Test on multiple screen sizes
   - Use relative units (%, em) where appropriate
   - Adjust label rotations for mobile

5. **Theming:**
   - Use Theme Studio for cohesive themes
   - Keep custom themes in separate files
   - Document custom color codes

## Advanced Customization

### Custom Series Rendering

```typescript
import { ChartComponent } from '@syncfusion/ej2-angular-charts';

export class CustomChartComponent {
  @ViewChild('chart') chart: ChartComponent;
  
  ngAfterViewInit() {
    // Access SVG elements
    let seriesElements = document.querySelectorAll('.e-series-0');
    seriesElements.forEach((element: HTMLElement) => {
      element.style.filter = 'drop-shadow(2px 2px 4px rgba(0,0,0,0.3))';
    });
  }
}
```

### Conditional Rendering

```typescript
seriesRender(args: ISeriesRenderEventArgs): void {
  if (args.series.name === 'Actual') {
    args.fill = this.actualColor;
  } else if (args.series.name === 'Forecast') {
    args.fill = this.forecastColor;
    args.dashArray = '5,5';
  }
}
```

## Troubleshooting

### Theme Not Applying

**Issue:** Theme styles not visible
**Solutions:**
- Verify CSS import order
- Check for CSS specificity conflicts
- Ensure theme CSS is loaded before component
- Clear browser cache

### Colors Not Changing

**Issue:** Custom colors not applied
**Solutions:**
- Check property names (case-sensitive)
- Verify color format (hex, rgb, rgba)
- Use `refresh()` after programmatic changes
- Check event timing (use `loaded` event)

### Performance Issues

**Issue:** Slow rendering with custom styles
**Solutions:**
- Move styles to CSS instead of inline
- Reduce DOM manipulations
- Use CSS classes instead of inline styles
- Disable animations for large datasets

---
