# Appearance and Styling

## Table of Contents
- [Color Palettes](#color-palettes)
- [Custom Colors](#custom-colors)
- [Themes](#themes)
- [Animation](#animation)
- [Gradients](#gradients)
- [Custom CSS Styling](#custom-css-styling)
- [Export and Print](#export-and-print)
- [Advanced Styling Examples](#advanced-styling-examples)

## Color Palettes

### Predefined Palettes

Use Syncfusion's built-in color palettes to quickly style your chart:

```typescript
<ejs-accumulationchart [palette]="'Tableau'">
  <e-accumulation-series [dataSource]="data">
  </e-accumulation-series>
</ejs-accumulationchart>
```

### Available Palette Options

| Palette | Use Case | Style |
|---------|----------|-------|
| `Material` | Modern, professional | Material Design colors |
| `Bootstrap` | Web applications | Bootstrap palette |
| `Fabric` | Microsoft Office style | Fabric UI colors |
| `Bootstrap4` | Bootstrap 4 theme | Bootstrap 4 colors |
| `Tailwind` | Tailwind CSS | Tailwind palette |
| `Tableau` | Business analytics | Tableau colors |
| `Highcontrast` | Accessibility | High contrast colors |

### Apply Palette

```typescript
@Component({
  template: `
    <ejs-accumulationchart [palette]="selectedPalette">
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  selectedPalette = 'Tableau'; // Material, Bootstrap, Fabric, etc.
}
```

## Custom Colors

### Custom Color Array

Define your own color scheme for chart segments:

```typescript
@Component({
  template: `
    <ejs-accumulationchart [palette]="customColors">
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  customColors = [
    '#FF6B6B',  // Red
    '#4ECDC4',  // Teal
    '#45B7D1',  // Blue
    '#FFA07A',  // Light Salmon
    '#98D8C8'   // Mint
  ];

  data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 },
    { x: 'C', y: 20 },
    { x: 'D', y: 15 },
    { x: 'E', y: 10 }
  ];
}
```

### Per-Point Custom Color

Style individual data points with custom colors:

```typescript
@Component({
  template: `
    <ejs-accumulationchart (pointRender)="onPointRender($event)">
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  data = [
    { x: 'A', y: 30, color: '#FF6B6B' },
    { x: 'B', y: 25, color: '#4ECDC4' },
    { x: 'C', y: 20, color: '#45B7D1' },
    { x: 'D', y: 15, color: '#FFA07A' }
  ];

  onPointRender(args: IAccumulationEventArgs) {
    // Apply custom color from data
    if (args.point.color) {
      args.fill = args.point.color;
    }
  }
}
```

### Point Border Styling

Add borders and customize segment edges:

```typescript
onPointRender(args: IAccumulationEventArgs) {
  args.border = {
    color: '#FFFFFF',    // White border
    width: 2
  };

  // Different border for specific points
  if (args.pointIndex === 0) {
    args.border.color = '#333';
    args.border.width = 3;
  }
}
```

## Themes

### Applying Themes

Syncfusion charts support multiple themes. Apply via CSS or programmatically:

### Available Themes

- **Material** - Google Material Design
- **Bootstrap** - Bootstrap theme
- **Fabric** - Microsoft Office style
- **Bootstrap4** - Bootstrap 4
- **Tailwind** - Tailwind CSS
- **Highcontrast** - High contrast for accessibility

### Theme Configuration

```typescript
<ejs-accumulationchart 
  theme="Material"
  [background]="'#f5f5f5'">
  <e-accumulation-series [dataSource]="data">
  </e-accumulation-series>
</ejs-accumulationchart>
```

## Right‑to‑Left (RTL) Support
The chart supports RTL mode for languages read right-to-left.

**API:** `enableRtl: boolean`

```typescript
// Enable RTL
import { enableRtl } from '@syncfusion/ej2-base';
enableRtl(true);
```

RTL affects:
- Legend alignment  
- Tooltip direction  
- Label flow  

## Animation

### Enable Animation

Animate chart segments on load:

```typescript
<ejs-accumulationchart>
  <e-accumulation-series
    [dataSource]="data"
    [animation]="{ enable: true, duration: 1000 }">
  </e-accumulation-series>
</ejs-accumulationchart>
```

### Animation Properties

```typescript
interface AnimationProperties {
  enable: boolean;           // Enable/disable animation
  duration: number;          // Animation duration in ms (default: 1000)
  delay: number;            // Delay before animation starts
  option: 'Rotate' | 'Zoom' | 'SlideForward'; // Animation type
}
```

### Animation Options

**Rotate:** Segments rotate into place
```typescript
[animation]="{ enable: true, option: 'Rotate', duration: 1500 }"
```

**Zoom:** Segments zoom into place
```typescript
[animation]="{ enable: true, option: 'Zoom', duration: 1000 }"
```

**SlideForward:** Segments slide into place
```typescript
[animation]="{ enable: true, option: 'SlideForward', duration: 1200 }"
```

### Delayed Animation

```typescript
@Component({
  template: `
    <ejs-accumulationchart>
      <e-accumulation-series
        [dataSource]="data"
        [animation]="animationConfig">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  animationConfig = {
    enable: true,
    duration: 1500,
    delay: 500,     // Wait 500ms before animating
    option: 'Zoom'
  };

  data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 }
  ];
}
```

## Gradients

### Linear Gradient Fill

Apply linear gradients to chart segments:

```typescript
onPointRender(args: IAccumulationEventArgs) {
  args.fill = new LinearGradient(
    {
      colors: ['#FF6B6B', '#FFA07A']
    }
  );
}
```

### Custom Gradient Configuration

```typescript
@Component({
  template: `
    <ejs-accumulationchart (pointRender)="onPointRender($event)">
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class GradientChartComponent {
  data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 },
    { x: 'C', y: 20 }
  ];

  onPointRender(args: IAccumulationEventArgs) {
    // Apply gradient based on index
    const colors = [
      { startColor: '#FF6B6B', endColor: '#FF8E8E' },
      { startColor: '#4ECDC4', endColor: '#6FE5D8' },
      { startColor: '#45B7D1', endColor: '#6ECDE8' }
    ];

    const color = colors[args.pointIndex % colors.length];
    args.fill = new LinearGradient(
      {
        colors: [color.startColor, color.endColor],
        angle: 90
      }
    );
  }
}
```

### Radial Gradient

For radial (circular) gradient effects:

```typescript
onPointRender(args: IAccumulationEventArgs) {
  args.fill = new RadialGradient(
    {
      colors: ['#FF6B6B', '#FFFFFF'],
      angle: 45
    }
  );
}
```

## Custom CSS Styling

### Chart Container Styling

```css
#container {
  height: 420px;
  width: 100%;
  border: 2px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
```

### Style Chart Elements

```typescript
titleStyle = {
  color: '#333',
  fontFamily: 'Segoe UI',
  fontSize: '18px',
  fontWeight: 'bold',
  backgroundColor: '#F0F0F0',
  padding: { top: 10, bottom: 10, left: 10, right: 10 },
  borderRadius: 5
};

labelStyle = {
  color: '#666',
  fontFamily: 'Arial',
  fontSize: '12px',
  fontStyle: 'italic'
};
```

### Apply Text Styles to Elements

```typescript
<ejs-accumulationchart 
  [titleStyle]="titleStyle"
  [subTitleStyle]="subtitleStyle">
  <e-accumulation-legend [textStyle]="legendTextStyle">
  </e-accumulation-legend>
  <e-accumulation-series
    [dataLabel]="{ textStyle: labelStyle }">
  </e-accumulation-series>
</ejs-accumulationchart>
```

## Chart Margin and Border

### Margin
Controls the outer spacing around the chart.

**API:**  
`margin: { left: number, right: number, top: number, bottom: number }`

```html
<ejs-accumulationchart
  [margin]="{ top: 20, bottom: 20 }">
</ejs-accumulationchart>
```

### Border
Adds a border around the entire chart area.

**API:**  
`border: { width: number, color: string }`

```html
<ejs-accumulationchart
  [border]="{ width: 1, color: '#ccc' }">
</ejs-accumulationchart>
```

## Export and Print

### Export to Image

Export chart as PNG, JPEG, SVG, or PDF:

```typescript
@Component({
  template: `
    <button (click)="exportChart()">Export as PNG</button>
    <ejs-accumulationchart #chart>
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ExportChartComponent implements ViewChild {
  @ViewChild('chart') chart!: any;

  exportChart() {
    this.chart.nativeElement.ej2_instances[0].export(
      'PNG',
      'accumulation-chart'
    );
  }

  data = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 }
  ];
}
```

### Export Types

```typescript
// PNG format
chart.export('PNG', 'chart-name');

// JPEG format
chart.export('JPEG', 'chart-name');

// SVG vector format
chart.export('SVG', 'chart-name');

// PDF format
chart.export('PDF', 'chart-name');
```

### Print Chart

```typescript
printChart() {
  const chartElement = document.getElementById('container');
  const printWindow = window.open('', '', 'height=500,width=500');
  printWindow!.document.write(chartElement!.innerHTML);
  printWindow!.print();
}
```

### Export Button Component

```typescript
@Component({
  selector: 'app-export-buttons',
  template: `
    <div class="export-buttons">
      <button (click)="exportPNG()" class="btn btn-primary">
        📥 Export PNG
      </button>
      <button (click)="exportSVG()" class="btn btn-secondary">
        📥 Export SVG
      </button>
      <button (click)="exportPDF()" class="btn btn-danger">
        📥 Export PDF
      </button>
      <button (click)="printChart()" class="btn btn-info">
        🖨️ Print
      </button>
    </div>
    
    <ejs-accumulationchart #chart>
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `,
  styles: [`
    .export-buttons { margin-bottom: 20px; }
    .btn { margin-right: 10px; padding: 8px 16px; }
  `]
})
export class ExportButtonsComponent {
  @ViewChild('chart') chart!: any;

  data = [
    { x: 'Q1', y: 40 },
    { x: 'Q2', y: 50 },
    { x: 'Q3', y: 60 },
    { x: 'Q4', y: 75 }
  ];

  exportPNG() {
    this.chart.ej2_instances[0].export('PNG', 'quarterly-sales');
  }

  exportSVG() {
    this.chart.ej2_instances[0].export('SVG', 'quarterly-sales');
  }

  exportPDF() {
    this.chart.ej2_instances[0].export('PDF', 'quarterly-sales');
  }

  printChart() {
    const chartHtml = document.getElementById('container')!.innerHTML;
    const printWindow = window.open('', '', 'height=500,width=800');
    printWindow!.document.write(`
      <html>
        <head><title>Chart</title></head>
        <body>${chartHtml}</body>
      </html>
    `);
    printWindow!.print();
  }
}
```

## Advanced Styling Examples

### Example 1: Dashboard Style with Multiple Charts

```typescript
@Component({
  selector: 'app-styled-dashboard',
  template: `
    <div class="dashboard-container">
      <div class="chart-card">
        <h3>Sales by Region</h3>
        <ejs-accumulationchart [palette]="['#FF6B6B', '#4ECDC4']">
          <e-accumulation-series [dataSource]="regionData" type="Pie">
          </e-accumulation-series>
        </ejs-accumulationchart>
      </div>
      
      <div class="chart-card">
        <h3>Product Distribution</h3>
        <ejs-accumulationchart [palette]="['#45B7D1', '#FFA07A', '#98D8C8']">
          <e-accumulation-series [dataSource]="productData" type="Doughnut">
          </e-accumulation-series>
        </ejs-accumulationchart>
      </div>
    </div>
  `,
  styles: [`
    .dashboard-container {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
      padding: 20px;
    }
    .chart-card {
      background: #fff;
      border-radius: 8px;
      padding: 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    .chart-card h3 {
      margin-top: 0;
      color: #333;
      font-size: 16px;
      font-weight: 600;
    }
  `]
})
export class StyledDashboardComponent {
  regionData = [
    { x: 'North', y: 45 },
    { x: 'South', y: 55 }
  ];

  productData = [
    { x: 'Product A', y: 35 },
    { x: 'Product B', y: 30 },
    { x: 'Product C', y: 35 }
  ];
}
```

### Example 2: Interactive Styling with State

```typescript
@Component({
  template: `
    <ejs-accumulationchart
      (pointRender)="onPointRender($event)"
      (pointClick)="onPointClick($event)">
      <e-accumulation-series [dataSource]="styledData">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class InteractiveStyledComponent {
  selectedIndex: number | null = null;

  styledData = [
    { x: 'A', y: 30 },
    { x: 'B', y: 25 },
    { x: 'C', y: 20 }
  ];

  onPointRender(args: IAccumulationEventArgs) {
    const isSelected = args.pointIndex === this.selectedIndex;
    
    if (isSelected) {
      args.fill = '#FF6B6B';
      args.border = { color: '#333', width: 3 };
    } else {
      args.fill = args.pointIndex % 2 === 0 ? '#4ECDC4' : '#45B7D1';
    }
  }

  onPointClick(args: IAccumulationEventArgs) {
    this.selectedIndex = args.pointIndex;
  }
}
```

## Key Takeaways

- **Palettes**: Use predefined or custom color schemes
- **Themes**: Apply Material, Bootstrap, Fabric, or Tailwind themes
- **Animation**: Enhance UX with rotation, zoom, or slide animations
- **Gradients**: Add visual depth with linear or radial gradients
- **Styling**: Customize fonts, colors, backgrounds for all elements
- **Export**: Support PNG, JPEG, SVG, PDF export and printing
- **Responsive**: Combine CSS media queries for mobile-friendly styling

---

## API Reference Summary

### Appearance APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `theme` | Built-in theme selection | [theme](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#theme) |
| `background` | Chart background color | [background](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#background) |
| `palettes` | Custom color palette | [palettes](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#palettes) |
| `border` | Chart border styling | [border](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#border) |
| `opacity` | Series opacity (0-1) | [opacity](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#opacity) |

### Animation APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `enableAnimation` | Enable chart animation | [enableAnimation](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#enableanimation) |
| `animation` | Animation settings | [animation](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationSeries#animation) |
| `AnimationModel` | Animation configuration interface | [AnimationModel](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/animationModel) |

### Export/Print APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `export()` | Export chart as image/PDF | [export](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#export) |
| `print()` | Print chart | [print](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#print) |

**For complete API documentation, see:** [api-reference.md](references/api-reference.md)