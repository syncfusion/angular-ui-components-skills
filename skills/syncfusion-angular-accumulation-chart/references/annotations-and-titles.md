# Annotations and Titles

## Table of Contents
- [Chart Titles](#chart-titles)
- [Subtitles](#subtitles)
- [Center Labels](#center-labels)
- [Annotations Overview](#annotations-overview)
- [Text Annotations](#text-annotations)
- [Image Annotations](#image-annotations)
- [Annotation Positioning](#annotation-positioning)
- [Advanced Annotation Examples](#advanced-annotation-examples)

## Chart Titles

### Basic Title

Add a main title to the accumulation chart:

```typescript
<ejs-accumulationchart [title]="'Sales Distribution Chart'">
  <e-accumulation-series [dataSource]="data">
  </e-accumulation-series>
</ejs-accumulationchart>
```

### Title Properties

```typescript
interface TitleProperties {
  text: string;                    // Title text content
  visible: boolean;                // Show/hide title
  textAlignment: 'Center' | 'Left' | 'Right'; // Horizontal alignment
  textStyle: object;               // Font, color, size, etc.
  enableTrim: boolean;             // Trim long titles
  maximumLabelWidth: number;       // Max width for title
  margin: object;                  // Title margins
  subtitle: object;                // Subtitle configuration
}
```

### Styled Title

```typescript
@Component({
  template: `
    <ejs-accumulationchart 
      [title]="'Revenue by Product'"
      [titleStyle]="titleStyle">
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  titleStyle = {
    color: '#333',
    fontFamily: 'Segoe UI',
    fontStyle: 'bold',
    fontSize: '18px',
    fontWeight: '500',
    textAlignment: 'Center',
    margin: { bottom: 20 }
  };
}
```

### Title with Border and Background

```typescript
titleStyle = {
  color: '#FFFFFF',
  backgroundColor: { color: '#3F51B5' },
  border: { color: '#000', width: 1 },
  padding: { bottom: 10, top: 10, left: 10, right: 10 },
  borderRadius: 5
};
```

## Subtitles

### Basic Subtitle

Add a subtitle below the main title:

```typescript
<ejs-accumulationchart 
  [title]="'Sales Analysis'"
  [subTitle]="'Q4 2024 Performance'">
  <e-accumulation-series [dataSource]="data">
  </e-accumulation-series>
</ejs-accumulationchart>
```

### Styled Subtitle

```typescript
@Component({
  template: `
    <ejs-accumulationchart 
      [title]="mainTitle"
      [subTitle]="subTitleText"
      [subTitleStyle]="subtitleStyle">
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class ChartComponent {
  mainTitle = 'Revenue Analysis';
  subTitleText = 'Fourth Quarter Results';
  
  subtitleStyle = {
    color: '#666',
    fontFamily: 'Arial',
    fontSize: '14px',
    fontStyle: 'italic',
    margin: { bottom: 10 }
  };
}
```

## Center Labels

### Center Label (Doughnut Only)

Display content in the hollow center of a doughnut chart:

```typescript
@Component({
  template: `
    <div class="chart-container">
      <ejs-accumulationchart id="container">
        <e-accumulation-series
          [dataSource]="data"
          xName="x"
          yName="y"
          type="Doughnut">
        </e-accumulation-series>
      </ejs-accumulationchart>
      
      <!-- Center label overlay -->
      <div class="center-label">
        <div class="metric">$156,000</div>
        <div class="label">Total Sales</div>
      </div>
    </div>
  `,
  styles: [`
    .chart-container {
      position: relative;
      height: 420px;
      width: 100%;
    }
    #container {
      height: 100%;
      width: 100%;
    }
    .center-label {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
      pointer-events: none;
    }
    .metric {
      font-size: 28px;
      font-weight: bold;
      color: #333;
    }
    .label {
      font-size: 14px;
      color: #999;
      margin-top: 5px;
    }
  `]
})
export class DoughnutComponent {
  data = [
    { x: 'Product A', y: 45000 },
    { x: 'Product B', y: 60000 },
    { x: 'Product C', y: 51000 }
  ];
}
```

### Dynamic Center Label with Calculation

```typescript
@Component({
  template: `
    <div class="chart-container">
      <ejs-accumulationchart id="container"
        (pointRender)="onPointRender($event)">
        <e-accumulation-series [dataSource]="data" type="Doughnut">
        </e-accumulation-series>
      </ejs-accumulationchart>
      
      <div class="center-label">
        <div class="metric">{{ totalValue | currency }}</div>
        <div class="label">Total</div>
        <div class="percentage">{{ percentageText }}</div>
      </div>
    </div>
  `,
  styles: [`
    .chart-container { position: relative; height: 420px; }
    .center-label { 
      position: absolute; 
      top: 50%; 
      left: 50%; 
      transform: translate(-50%, -50%);
      text-align: center;
    }
  `]
})
export class DynamicCenterComponent implements OnInit {
  data = [
    { x: 'North', y: 50000 },
    { x: 'South', y: 70000 },
    { x: 'East', y: 60000 }
  ];
  
  totalValue = 0;
  percentageText = '100%';

  ngOnInit() {
    this.calculateTotal();
  }

  calculateTotal() {
    this.totalValue = this.data.reduce((sum, item) => sum + item.y, 0);
  }

  onPointRender(args: any) {
    // Optional: customize point rendering
  }
}
```

### Chart Center Positioning
You can reposition the chart using center coordinates.

**APIs:**
- `center: { x: string | number, y: string | number }`
- `centerX: string | number`
- `centerY: string | number`

```html
<ejs-accumulationchart center="20%, 40%">
</ejs-accumulationchart>
```

This is useful for custom layouts, overlay text, and off-centered doughnut designs.

## Annotations Overview

### What Are Annotations?

Annotations are text, shapes, or images placed on the chart to highlight or explain specific areas or data points.

### Types of Annotations

1. **Text Annotations** - Add text labels or notes
2. **Image Annotations** - Add images to chart area
3. **Positional Annotations** - Place annotations at specific coordinates

## Text Annotations

### Basic Text Annotation

```typescript
interface TextAnnotation {
  content: string | HTMLElement;     // Text or HTML content
  x: string | number;                // X position (pixel or percentage)
  y: string | number;                // Y position
  coordinateUnits: 'Pixel' | 'Point'; // Position reference
  region: 'Series' | 'Chart';        // Placement area
  horizontalAlignment: 'Left' | 'Center' | 'Right';
  verticalAlignment: 'Top' | 'Middle' | 'Bottom';
  enableAnimation: boolean;
  textStyle: object;                 // Font styling
}
```

### Simple Text Annotation

```typescript
<ejs-accumulationchart>
  <e-accumulation-annotations>
    <e-annotation 
      content="Peak Sales"
      x="50%"
      y="25%"
      coordinateUnits="Pixel"
      horizontalAlignment="Center">
    </e-annotation>
  </e-accumulation-annotations>
  
  <e-accumulation-series [dataSource]="data">
  </e-accumulation-series>
</ejs-accumulationchart>
```

### Styled Text Annotation

```typescript
@Component({
  template: `
    <ejs-accumulationchart>
      <e-accumulation-annotations>
        <e-annotation 
          [content]="annotationContent"
          x="50%"
          y="30%"
          [textStyle]="annotationStyle"
          horizontalAlignment="Center">
        </e-annotation>
      </e-accumulation-annotations>
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class AnnotatedChartComponent {
  annotationContent = '⭐ Q4 Peak';
  
  annotationStyle = {
    color: '#FF6B6B',
    fontFamily: 'Segoe UI',
    fontSize: '14px',
    fontWeight: 'bold',
    backgroundColor: '#FFFFCC',
    padding: { top: 5, bottom: 5, left: 10, right: 10 },
    borderRadius: 5
  };

  data = [
    { x: 'Q1', y: 30000 },
    { x: 'Q4', y: 95000 }
  ];
}
```

## Image Annotations

### Add Image Annotation

```typescript
<ejs-accumulationchart>
  <e-accumulation-annotations>
    <e-annotation 
      content="<img src='assets/star.png' width='30' height='30'>"
      x="50%"
      y="20%"
      coordinateUnits="Pixel"
      horizontalAlignment="Center">
    </e-annotation>
  </e-accumulation-annotations>
  
  <e-accumulation-series [dataSource]="data">
  </e-accumulation-series>
</ejs-accumulationchart>
```

### Image Annotation Component

```typescript
@Component({
  template: `
    <ejs-accumulationchart>
      <e-accumulation-annotations>
        <e-annotation 
          *ngFor="let ann of annotations"
          [content]="ann.content"
          [x]="ann.x"
          [y]="ann.y"
          coordinateUnits="Pixel"
          horizontalAlignment="Center">
        </e-annotation>
      </e-accumulation-annotations>
      <e-accumulation-series [dataSource]="data">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `
})
export class AnnotatedImageChartComponent {
  annotations = [
    {
      content: '<img src="assets/star.png" width="30">',
      x: '50%',
      y: '15%'
    },
    {
      content: '<img src="assets/arrow.png" width="40">',
      x: '75%',
      y: '40%'
    }
  ];

  data = [
    { x: 'A', y: 25 },
    { x: 'B', y: 65 },
    { x: 'C', y: 40 }
  ];
}
```

## Annotation Positioning

### Coordinate Units

**Pixel:** Position relative to chart container

```typescript
<e-annotation 
  content="Pixel Positioned"
  x="100"
  y="150"
  coordinateUnits="Pixel">
</e-annotation>
```

**Point:** Position relative to data coordinates (if applicable)

```typescript
<e-annotation 
  content="Point Positioned"
  x="2"
  y="1500"
  coordinateUnits="Point">
</e-annotation>
```

### Alignment Options

**Horizontal Alignment:**
- `Left` - Align left
- `Center` - Center horizontally (default)
- `Right` - Align right

**Vertical Alignment:**
- `Top` - Align to top
- `Middle` - Center vertically (default)
- `Bottom` - Align to bottom

### Combined Positioning Example

```typescript
<ejs-accumulationchart>
  <e-accumulation-annotations>
    <!-- Top-left -->
    <e-annotation 
      content="Top Left"
      x="10"
      y="10"
      coordinateUnits="Pixel"
      horizontalAlignment="Left"
      verticalAlignment="Top">
    </e-annotation>
    
    <!-- Center -->
    <e-annotation 
      content="Center"
      x="50%"
      y="50%"
      coordinateUnits="Pixel"
      horizontalAlignment="Center"
      verticalAlignment="Middle">
    </e-annotation>
    
    <!-- Bottom-right -->
    <e-annotation 
      content="Bottom Right"
      x="95%"
      y="95%"
      coordinateUnits="Pixel"
      horizontalAlignment="Right"
      verticalAlignment="Bottom">
    </e-annotation>
  </e-accumulation-annotations>
  
  <e-accumulation-series [dataSource]="data">
  </e-accumulation-series>
</ejs-accumulationchart>
```

## Advanced Annotation Examples

### Example 1: KPI Dashboard with Annotations

```typescript
@Component({
  selector: 'app-kpi-chart',
  template: `
    <ejs-accumulationchart 
      id="container"
      [title]="'Sales Performance KPI'"
      [subTitle]="'Quarterly Comparison'">
      
      <e-accumulation-annotations>
        <e-annotation 
          content="<div class='kpi-annotation'><span>$450K</span></div>"
          x="30%"
          y="10%"
          coordinateUnits="Pixel">
        </e-annotation>
        
        <e-annotation 
          content="<div class='kpi-annotation success'>↑ 23%</div>"
          x="70%"
          y="10%"
          coordinateUnits="Pixel">
        </e-annotation>
      </e-accumulation-annotations>
      
      <e-accumulation-series
        [dataSource]="kpiData"
        xName="quarter"
        yName="sales"
        type="Pie"
        [dataLabel]="{ visible: true }">
      </e-accumulation-series>
    </ejs-accumulationchart>
  `,
  styles: [`
    #container { height: 420px; }
    .kpi-annotation {
      background: #F0F0F0;
      padding: 10px 15px;
      border-radius: 5px;
      font-weight: bold;
    }
    .kpi-annotation.success {
      background: #D4EDDA;
      color: #155724;
    }
  `]
})
export class KPIDashboardComponent {
  kpiData = [
    { quarter: 'Q1', sales: 150000 },
    { quarter: 'Q2', sales: 200000 },
    { quarter: 'Q3', sales: 250000 },
    { quarter: 'Q4', sales: 310000 }
  ];
}
```

### Example 2: Trend Annotation on Doughnut

```typescript
@Component({
  template: `
    <div class="chart-container">
      <ejs-accumulationchart id="container">
        <e-accumulation-annotations>
          <e-annotation 
            *ngIf="showTrendAnnotation"
            [content]="trendContent"
            x="50%"
            y="40%"
            coordinateUnits="Pixel"
            [textStyle]="trendStyle">
          </e-annotation>
        </e-accumulation-annotations>
        
        <e-accumulation-series
          [dataSource]="monthlyData"
          xName="month"
          yName="revenue"
          type="Doughnut">
        </e-accumulation-series>
      </ejs-accumulationchart>
    </div>
  `
})
export class TrendAnnotationComponent {
  showTrendAnnotation = true;
  trendContent = '📈 Uptrend';
  trendStyle = { color: '#28A745', fontSize: '16px', fontWeight: 'bold' };
  
  monthlyData = [
    { month: 'Jan', revenue: 45000 },
    { month: 'Feb', revenue: 52000 },
    { month: 'Mar', revenue: 68000 }
  ];
}
```

## Key Takeaways

- **Titles & Subtitles**: Provide context and clarity to charts
- **Center Labels**: Use with doughnut charts for central KPI display
- **Text Annotations**: Add explanatory notes or callouts
- **Image Annotations**: Highlight important areas with visual markers
- **Positioning**: Use pixel or point coordinates for precise placement
- **Styling**: Customize fonts, colors, and backgrounds for visual hierarchy
- **Accessibility**: Ensure annotations are readable and don't obscure data

---

## API Reference Summary

### Annotation APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `AccumulationAnnotationSettings` | Annotation configuration model | [AccumulationAnnotationSettings](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationAnnotationSettings) |
| `content` | Annotation HTML content | [content](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationAnnotationSettings#content) |
| `region` | Annotation region | [region](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationAnnotationSettings#region) |
| `x` | X position | [x](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationAnnotationSettings#x) |
| `y` | Y position | [y](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationAnnotationSettings#y) |

### Title APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `title` | Chart title text | [title](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#title) |
| `titleStyle` | Title font styling | [titleStyle](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#titlestyle) |
| `subTitle` | Chart subtitle text | [subTitle](https://ej2.syncfusion.com/angular/documentation/api/accumulation-chart/accumulationChart#subtitle) |

**For complete API documentation, see:** [api-reference.md](references/api-reference.md)