# Getting Started with Syncfusion Angular Chart

This guide covers the complete setup process for integrating the Syncfusion Angular Chart component into your Angular application, from installation through creating your first chart.

## Prerequisites

### System Requirements

Before starting, ensure your development environment meets these requirements:

- **Node.js:** Version 18.19 or later
- **npm:** Version 10.x or later
- **Angular CLI:** Version 19 or later (supports Angular 21)
- **Supported Browsers:** Chrome, Firefox, Safari, Edge (latest versions)

### Angular Version Compatibility

The Syncfusion Angular Chart component supports Angular 19, 20, 21, and other recent versions. For detailed compatibility information, refer to the [Angular version support matrix](https://ej2.syncfusion.com/angular/documentation/system-requirement#angular-version-compatibility).

**Note:** Starting from Angular 19, standalone components are the default architecture. This guide uses the modern standalone approach.

## Setup Angular Environment

### Install Angular CLI

If you haven't installed Angular CLI globally, run:

```bash
npm install -g @angular/cli
```

To install a specific version:

```bash
npm install -g @angular/cli@21.0.0
```

### Create a New Angular Application

Generate a new Angular application:

```bash
ng new my-chart-app
```

During setup, you'll be prompted for configuration:

**Stylesheet Format:**
```
? Which stylesheet format would you like to use?
> CSS
  Sass (SCSS)
  Sass (Indented)
  Less
```

**Server-Side Rendering (SSR):**
Choose based on your requirements. For client-side only applications, select "No".

**AI Tool Integration:**
Select your preferred AI tool or choose "none".

Navigate to your project:

```bash
cd my-chart-app
```

## Installing Syncfusion Chart Package

### Using ng add (Recommended)

The simplest way to add the Chart component:

```bash
ng add @syncfusion/ej2-angular-charts
```

This command automatically:
- Adds `@syncfusion/ej2-angular-charts` package and peer dependencies to `package.json`
- Imports the Chart module in your application
- Configures necessary dependencies

### Manual Installation

Alternatively, install manually using npm:

```bash
npm install @syncfusion/ej2-angular-charts --save
```

## Registering Syncfusion License

Syncfusion is a commercial library requiring a valid license. Register your license key to avoid watermarks:

### Get a License Key

1. **Free Trial:** Get a 30-day trial at [syncfusion.com](https://www.syncfusion.com/downloads)
2. **Community License:** Free for qualifying individuals/organizations
3. **Commercial License:** For commercial projects

### Register the License

**For Angular 19+ (with app.config.ts):**

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { registerLicense } from '@syncfusion/ej2-base';

// Register Syncfusion license key
registerLicense('YOUR_LICENSE_KEY_HERE');

export const appConfig: ApplicationConfig = {
  providers: []
};
```

**For Angular 18 and below (with main.ts):**

```typescript
// main.ts
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';
import { registerLicense } from '@syncfusion/ej2-base';

// Register Syncfusion license key
registerLicense('YOUR_LICENSE_KEY_HERE');

bootstrapApplication(AppComponent);
```

## Adding CSS Themes

Syncfusion components require CSS themes for styling. Add theme imports to your global styles file.

### Available Themes

- **Material:** Material Design theme
- **Bootstrap:** Bootstrap theme (4 and 5)
- **Fabric:** Microsoft Fabric theme
- **Tailwind:** Tailwind CSS theme
- **Bootstrap 5:** Bootstrap 5 design
- **Fluent:** Microsoft Fluent theme
- **Material 3:** Material Design 3

### Import Theme CSS

**Option 1: Add to styles.css (or styles.scss)**

```css
/* styles.css */
@import '../node_modules/@syncfusion/ej2-base/styles/material.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/material.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material.css';
@import '../node_modules/@syncfusion/ej2-angular-charts/styles/material.css';
```

**Option 2: Add to angular.json**

```json
{
  "projects": {
    "my-chart-app": {
      "architect": {
        "build": {
          "options": {
            "styles": [
              "node_modules/@syncfusion/ej2-base/styles/material.css",
              "node_modules/@syncfusion/ej2-buttons/styles/material.css",
              "node_modules/@syncfusion/ej2-popups/styles/material.css",
              "node_modules/@syncfusion/ej2-angular-charts/styles/material.css",
              "src/styles.css"
            ]
          }
        }
      }
    }
  }
}
```

**For Bootstrap theme, use:**
```css
@import '../node_modules/@syncfusion/ej2-base/styles/bootstrap5.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/bootstrap5.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/bootstrap5.css';
@import '../node_modules/@syncfusion/ej2-angular-charts/styles/bootstrap5.css';
```

## Creating Your First Chart

### Basic Line Chart Example

**For Angular 20+ (app.ts structure):**

```typescript
// app.ts
import { Component } from '@angular/core';
import { ChartModule } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ChartModule],
  template: `
    <div style="padding: 20px;">
      <h2>Sales Report</h2>
      <ejs-chart 
        id="chart-container"
        [primaryXAxis]="primaryXAxis"
        [primaryYAxis]="primaryYAxis"
        [title]="title">
        <e-series-collection>
          <e-series 
            [dataSource]="chartData" 
            type="Line" 
            xName="month" 
            yName="sales"
            name="Sales"
            width="2">
          </e-series>
        </e-series-collection>
      </ejs-chart>
    </div>
  `,
  styles: [`
    #chart-container {
      height: 400px;
      width: 100%;
    }
  `]
})
export class App {
  public primaryXAxis = {
    valueType: 'Category',
    title: 'Month'
  };
  
  public primaryYAxis = {
    title: 'Sales (in USD)',
    labelFormat: '${value}K'
  };
  
  public title = 'Monthly Sales Analysis';
  
  public chartData = [
    { month: 'Jan', sales: 35 },
    { month: 'Feb', sales: 28 },
    { month: 'Mar', sales: 34 },
    { month: 'Apr', sales: 32 },
    { month: 'May', sales: 40 },
    { month: 'Jun', sales: 32 },
    { month: 'Jul', sales: 35 },
    { month: 'Aug', sales: 55 },
    { month: 'Sep', sales: 38 },
    { month: 'Oct', sales: 30 },
    { month: 'Nov', sales: 25 },
    { month: 'Dec', sales: 32 }
  ];
}

export default App;
```

**For Angular 19 and below (app.component.ts structure):**

```typescript
// app.component.ts
import { Component } from '@angular/core';
import { ChartModule } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ChartModule],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  public primaryXAxis = {
    valueType: 'Category',
    title: 'Month'
  };
  
  public primaryYAxis = {
    title: 'Sales (in USD)',
    labelFormat: '${value}K'
  };
  
  public title = 'Monthly Sales Analysis';
  
  public chartData = [
    { month: 'Jan', sales: 35 },
    { month: 'Feb', sales: 28 },
    { month: 'Mar', sales: 34 },
    { month: 'Apr', sales: 32 },
    { month: 'May', sales: 40 },
    { month: 'Jun', sales: 32 }
  ];
}
```

```html
<!-- app.component.html -->
<div style="padding: 20px;">
  <h2>Sales Report</h2>
  <ejs-chart 
    id="chart-container"
    [primaryXAxis]="primaryXAxis"
    [primaryYAxis]="primaryYAxis"
    [title]="title">
    <e-series-collection>
      <e-series 
        [dataSource]="chartData" 
        type="Line" 
        xName="month" 
        yName="sales"
        name="Sales">
      </e-series>
    </e-series-collection>
  </ejs-chart>
</div>
```

```css
/* app.component.css */
#chart-container {
  height: 400px;
  width: 100%;
}
```

## Running the Application

Start the development server:

```bash
ng serve
```

Or specify a port:

```bash
ng serve --port 4200
```

Open your browser and navigate to:
```
http://localhost:4200
```

You should see your chart rendered with the sample data.

## Understanding the Chart Structure

### Key Components

**1. Chart Container (`<ejs-chart>`)**
- Main chart component
- Contains all chart configuration
- API Reference: See [ChartModel](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel) for all available properties

**2. Series Collection (`<e-series-collection>`)**
- Container for one or more series
- Each series represents a data set
- API Reference: See [SeriesDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective)

**3. Individual Series (`<e-series>`)**
- Defines data source, type, and configuration
- Multiple series can be added for comparison
- API Reference: See [Series](https://ej2.syncfusion.com/angular/documentation/api/chart/series) for all series properties

### Essential Properties

**Chart Level Properties:**

| Property | Type | Description | API Reference |
|----------|------|-------------|---------------|
| `primaryXAxis` | AxisModel | X-axis configuration including valueType, title, range, labels | [AxisModel](https://ej2.syncfusion.com/angular/documentation/api/chart/axisModel) |
| `primaryYAxis` | AxisModel | Y-axis configuration including title, range, format, intervals | [AxisModel](https://ej2.syncfusion.com/angular/documentation/api/chart/axisModel) |
| `title` | string | Chart title text displayed at the top | [ChartModel.title](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#title) |
| `width` | string | Chart width (e.g., '100%', '800px') | [ChartModel.width](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#width) |
| `height` | string | Chart height (e.g., '400px') | [ChartModel.height](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#height) |
| `dataSource` | Object[] \| DataManager | Data source for the chart | [ChartModel.dataSource](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#dataSource) |
| `tooltip` | TooltipSettingsModel | Tooltip configuration | [TooltipSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipSettingsModel) |
| `legend` | LegendSettingsModel | Legend configuration | [LegendSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettingsModel) |

**Series Level Properties:**

| Property | Type | Default | Description | API Reference |
|----------|------|---------|-------------|---------------|
| `dataSource` | Object[] \| DataManager | '' | Data array or DataManager instance | [Series.dataSource](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#dataSource) |
| `type` | ChartSeriesType | 'Line' | Chart type: Line, Column, Bar, Area, Pie, etc. | [Series.type](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#type) |
| `xName` | string | '' | Property name for x-axis values in data objects | [Series.xName](https://ej2.syncfusion.com/angular/documentation/api/chart/series#xName) |
| `yName` | string | '' | Property name for y-axis values in data objects | [Series.yName](https://ej2.syncfusion.com/angular/documentation/api/chart/series#yName) |
| `name` | string | '' | Series name displayed in legend and tooltip | [Series.name](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#name) |
| `width` | number | 2 | Line width for line-based series | [Series.width](https://ej2.syncfusion.com/angular/documentation/api/chart/series#width) |
| `fill` | string | null | Series fill color | [Series.fill](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#fill) |
| `opacity` | number | 1 | Series opacity (0 to 1) | [Series.opacity](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#opacity) |
| `marker` | MarkerSettingsModel | - | Marker configuration for data points | [MarkerSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettingsModel) |

## Common Initial Configurations

### Setting Chart Dimensions

The chart component supports flexible sizing through `width` and `height` properties.

**API Reference:** [ChartModel.width](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#width), [ChartModel.height](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#height)

```typescript
// Fixed dimensions
<ejs-chart width="800px" height="400px">
</ejs-chart>

// Responsive (percentage)
<ejs-chart width="100%" height="350px">
</ejs-chart>
```

### Adding Multiple Series

Multiple series allow comparison of different data sets on the same chart.

**API Reference:** [SeriesDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective), [Series.name](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective#name)

```typescript
public data1 = [{ x: 'Jan', y: 35 }, { x: 'Feb', y: 28 }];
public data2 = [{ x: 'Jan', y: 25 }, { x: 'Feb', y: 33 }];
```

```html
<ejs-chart>
  <e-series-collection>
    <e-series [dataSource]="data1" type="Column" xName="x" yName="y" name="Product A"></e-series>
    <e-series [dataSource]="data2" type="Column" xName="x" yName="y" name="Product B"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Basic Tooltip

Enable tooltips to display data point information on hover.

**API Reference:** [TooltipSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipSettingsModel)

**Key Properties:**
- `enable` (boolean, default: false): Enable/disable tooltip
- `format` (string): Customize tooltip display format
- `shared` (boolean, default: false): Show all series data in one tooltip

```typescript
public tooltip = { 
  enable: true,
  format: '${point.x}: ${point.y}',
  shared: false
};
```

```html
<ejs-chart [tooltip]="tooltip">
  <!-- series -->
</ejs-chart>
```

## Troubleshooting

### Chart Not Rendering

**Issue:** Blank space where chart should appear

**Solutions:**
1. Verify CSS theme is imported
2. Check chart container has height defined
3. Ensure license is registered (check browser console for warnings)
4. Verify data source has valid data

### License Warning/Watermark

**Issue:** "Trial Expired" or watermark appears on chart

**Solution:**
- Register valid license key using `registerLicense()` in app.config.ts or main.ts
- Get trial license from syncfusion.com

### Module Not Found Error

**Issue:** `Cannot find module '@syncfusion/ej2-angular-charts'`

**Solution:**
```bash
npm install @syncfusion/ej2-angular-charts --save
```

### Build Errors with Standalone Components

**Issue:** Component not recognized

**Solution:**
- Ensure `ChartModule` is imported in the `imports` array
- Verify `standalone: true` is set in component decorator

## API Reference Summary

This section provides quick links to the most commonly used APIs when getting started with Syncfusion Angular Charts.

### Core Interfaces and Classes

| API | Description | Documentation |
|-----|-------------|---------------|
| ChartModel | Main chart component configuration with 40+ properties | [chartModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel) |
| AxisModel | Axis configuration with 50+ properties for X/Y axes | [axisModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/axisModel) |
| SeriesDirective | Series configuration with 60+ properties | [seriesDirective.md](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective) |
| TooltipSettingsModel | Tooltip appearance and behavior | [tooltipSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipSettingsModel) |
| LegendSettingsModel | Legend configuration | [legendSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettingsModel) |
| MarkerSettingsModel | Data point marker configuration | [markerSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettingsModel) |
| DataLabelSettingsModel | Data label configuration | [dataLabelSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelSettingsModel) |

### Essential Events

| Event | Interface | Description | API Reference |
|-------|-----------|-------------|---------------|
| `load` | ILoadedEventArgs | Triggered before chart rendering | [ChartModel.load](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#load) |
| `loaded` | ILoadedEventArgs | Triggered after chart loads | [ChartModel.loaded](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#loaded) |
| `pointRender` | IPointRenderEventArgs | Triggered before rendering each point | [IPointRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPointRenderEventArgs) |
| `seriesRender` | ISeriesRenderEventArgs | Triggered before rendering each series | [ISeriesRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iSeriesRenderEventArgs) |
| `tooltipRender` | ITooltipRenderEventArgs | Triggered before tooltip rendering | [ITooltipRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iTooltipRenderEventArgs) |
| `axisLabelRender` | IAxisLabelRenderEventArgs | Triggered before axis label rendering | [IAxisLabelRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iAxisLabelRenderEventArgs) |

### Enumerations

| Enum | Description | API Reference |
|------|-------------|---------------|
| ChartSeriesType | Available series types (Line, Column, Bar, Area, etc.) | [chartSeriesType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartSeriesType) |
| ValueType | Axis value types (Double, DateTime, Category, Logarithmic) | [valueType.md](https://ej2.syncfusion.com/angular/documentation/api/chart/valueType) |
| ChartTheme | Available themes (Material, Bootstrap, Fabric, etc.) | [chartTheme.md](https://ej2.syncfusion.com/angular/documentation/api/chart/chartTheme) |
| LegendPosition | Legend position options | [legendPosition.md](https://ej2.syncfusion.com/angular/documentation/api/chart/legendPosition) |

## Next Steps

Now that you have a basic chart running:

1. **Explore Series Types:** Try different chart types (Column, Bar, Area, Pie) by changing the `type` property - See [series-types.md](./series-types.md)
2. **Add Interactivity:** Enable tooltips, zooming, and crosshair features - See [interactive-features.md](./interactive-features.md)
3. **Customize Appearance:** Apply different themes and customize colors - See [customization.md](./customization.md)
4. **Bind Dynamic Data:** Connect to APIs or services for real-time data - See [data-binding.md](./data-binding.md)
5. **Add Chart Elements:** Include legends, data labels, and markers - See [chart-elements.md](./chart-elements.md)
6. **Configure Axes:** Customize axis properties and layout - See [axes-and-layout.md](./axes-and-layout.md)

Refer to the other reference files for detailed guidance on each feature area.
