# Getting Started with Angular Maps

Complete guide to installing, configuring, and creating your first Syncfusion Angular Maps component.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Dependencies](#dependencies)
- [Setup Angular Environment](#setup-angular-environment)
- [Installation](#installation)
- [Adding Maps Component](#adding-maps-component)
- [Module Injection](#module-injection)
- [Loading GeoJSON Shape Data](#loading-geojson-shape-data)
- [Binding Data Source](#binding-data-source)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (LTS version recommended)
- **npm** (Node Package Manager)
- **Angular CLI** (Command Line Interface)

Check your versions:
```bash
node --version   # Should be v14.x or higher
npm --version    # Should be v6.x or higher
```

## Dependencies

The Syncfusion Angular Maps component has the following dependency structure:

```
|-- @syncfusion/ej2-angular-maps
    |-- @syncfusion/ej2-angular-base
    |-- @syncfusion/ej2-maps
    |-- @syncfusion/ej2-base
    |-- @syncfusion/ej2-svg-base
    |-- @syncfusion/ej2-data
```

All dependencies are automatically installed when you install the main package.

## Setup Angular Environment

### Install Angular CLI

If you haven't already, install the Angular CLI globally:

```bash
npm install -g @angular/cli
```

### Create a New Angular Application

Create a new Angular project:

```bash
ng new my-maps-app
cd my-maps-app
```

When prompted:
- **Would you like to add Angular routing?** Choose based on your needs (Yes/No)
- **Which stylesheet format would you like to use?** Choose CSS, SCSS, SASS, or LESS

## Installation

Syncfusion provides two package formats for Angular components:

### Option 1: Ivy Library Distribution (Recommended)

For **Angular 12 and later versions**, use the Ivy distribution package:

```bash
npm install @syncfusion/ej2-angular-maps --save
```

This is the modern, optimized package format compatible with Angular's Ivy rendering engine.

### Option 2: Angular Compatibility Compiler (Legacy)

For **Angular versions earlier than 12**, use the ngcc package:

```bash
npm install @syncfusion/ej2-angular-maps@ngcc --save
```

In your `package.json`, the ngcc package appears as:
```json
{
  "dependencies": {
    "@syncfusion/ej2-angular-maps": "32.1.19-ngcc"
  }
}
```

**Important:** If you don't specify the `-ngcc` suffix for older Angular versions, you may encounter compatibility warnings.

## Adding Maps Component

### Step 1: Import MapsModule

For **standalone components** (Angular 14+):

```typescript
import { Component } from '@angular/core';
import { MapsModule } from '@syncfusion/ej2-angular-maps';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [MapsModule],
  template: `<ejs-maps id='maps-container'></ejs-maps>`
})
export class AppComponent { }
```

For **module-based applications**:

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { MapsModule } from '@syncfusion/ej2-angular-maps';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, MapsModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Step 2: Add Basic Maps Template

Modify your component template to include the Maps element:

```typescript
import { Component } from '@angular/core';
import { MapsModule } from '@syncfusion/ej2-angular-maps';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [MapsModule],
  template: `
    <ejs-maps id='maps-container'>
      <e-layers>
        <e-layer></e-layer>
      </e-layers>
    </ejs-maps>
  `
})
export class AppComponent { }
```

### Step 3: Add CSS Theme

Import Syncfusion CSS in your `styles.css` or `styles.scss`:

```css
/* Material theme */
@import '@syncfusion/ej2-base/styles/material.css';
@import '@syncfusion/ej2-angular-maps/styles/material.css';
```

**Available themes:**
- Material: `material.css`
- Bootstrap: `bootstrap.css` or `bootstrap5.css`
- Fabric: `fabric.css`
- Tailwind: `tailwind.css`
- Material 3: `material3.css`
- Fluent: `fluent.css`
- High Contrast: `highcontrast.css`

## Module Injection

The Maps component uses feature-specific services that must be injected when using those features:

```typescript
import { Component } from '@angular/core';
import { 
  MapsModule,
  MarkerService,          // For markers
  BubbleService,          // For bubbles
  DataLabelService,       // For data labels
  LegendService,          // For legends
  MapsTooltipService,     // For tooltips
  ZoomService,            // For zoom and pan
  SelectionService,       // For selection
  HighlightService,       // For highlighting
  NavigationLineService,  // For navigation lines
  AnnotationsService,     // For annotations
  PolygonService          // For polygons
} from '@syncfusion/ej2-angular-maps';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [MapsModule],
  providers: [
    LegendService,
    DataLabelService,
    MapsTooltipService
  ],
  template: `...`
})
export class AppComponent { }
```

**Important:** Only inject the services you actually use to minimize bundle size.

## Loading GeoJSON Shape Data

### Step 1: Obtain GeoJSON Data

You need GeoJSON data to render map shapes. Common sources:

1. **Syncfusion's World Map Data:**
   - Download from: https://www.syncfusion.com/downloads/support/directtrac/general/ze/world-map-2091224620
   - Place in your project (e.g., `src/app/world-map.ts`)

2. **Other GeoJSON Sources:**
   - [Natural Earth Data](https://www.naturalearthdata.com/)
   - [DataHub.io](https://datahub.io/core/geo-countries)
   - [GeoJSON.xyz](https://geojson.xyz/)

### Step 2: Import GeoJSON Data

Create a TypeScript file for your GeoJSON data:

```typescript
// world-map.ts
export let world_map: object = {
  "type": "FeatureCollection",
  "crs": {
    "type": "name",
    "properties": {
      "name": "urn:ogc:def:crs:OGC:1.3:CRS84"
    }
  },
  "features": [
    {
      "type": "Feature",
      "properties": {
        "name": "United States"
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[-100, 40], [-105, 35], [-95, 35], [-100, 40]]]
      }
    }
    // ... more features
  ]
};
```

### Step 3: Bind Shape Data

Import and bind the GeoJSON data to the layer's `shapeData` property:

```typescript
import { Component } from '@angular/core';
import { MapsModule } from '@syncfusion/ej2-angular-maps';
import { world_map } from './world-map';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [MapsModule],
  template: `
    <ejs-maps id='maps-container'>
      <e-layers>
        <e-layer [shapeData]='shapeData'></e-layer>
      </e-layers>
    </ejs-maps>
  `
})
export class AppComponent {
  public shapeData: object = world_map;
}
```

## Binding Data Source

To display data-driven visualizations, bind external data to shapes:

```typescript
import { Component } from '@angular/core';
import { MapsModule, LegendService } from '@syncfusion/ej2-angular-maps';
import { world_map } from './world-map';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [MapsModule],
  providers: [LegendService],
  template: `
    <ejs-maps id='maps-container' [legendSettings]='legendSettings'>
      <e-layers>
        <e-layer 
          [shapeData]='shapeData'
          [dataSource]='dataSource'
          shapeDataPath='country'
          shapePropertyPath='name'
          [shapeSettings]='shapeSettings'>
        </e-layer>
      </e-layers>
    </ejs-maps>
  `
})
export class AppComponent {
  public shapeData: object = world_map;
  
  // External data source
  public dataSource: object[] = [
    { country: 'United States', population: 331002651, candidate: 'Trump' },
    { country: 'Canada', population: 37742154, candidate: 'Clinton' },
    { country: 'Mexico', population: 128932753, candidate: 'Clinton' }
  ];
  
  // Shape settings with color mapping
  public shapeSettings: object = {
    colorValuePath: 'candidate',
    colorMapping: [
      { value: 'Trump', color: '#D84444' },
      { value: 'Clinton', color: '#316DB5' }
    ]
  };
  
  // Legend configuration
  public legendSettings: object = {
    visible: true,
    position: 'Bottom'
  };
}
```

**Key Properties:**
- `dataSource` - Array of data objects
- `shapeDataPath` - Field in dataSource that identifies the shape (e.g., 'country')
- `shapePropertyPath` - Field in shapeData (GeoJSON) that matches shapeDataPath (e.g., 'name')
- `colorValuePath` - Field in dataSource used for color mapping

## Running the Application

### Development Server

Start the Angular development server:

```bash
npm start
# or
ng serve
```

The application will be available at `http://localhost:4200/`

### Build for Production

Create an optimized production build:

```bash
ng build --configuration production
```

The build artifacts will be in the `dist/` directory.

## Troubleshooting

### Issue: Maps Not Displaying

**Symptoms:** Blank screen or empty container

**Solutions:**
1. **Check CSS import:** Ensure you've imported the Syncfusion CSS theme in `styles.css`
2. **Verify shapeData:** Ensure GeoJSON data is properly imported and bound
3. **Check container height:** Add explicit height to the maps container:
   ```css
   #maps-container {
     height: 500px;
     width: 100%;
   }
   ```

### Issue: Module Not Found Error

**Symptoms:** `Cannot find module '@syncfusion/ej2-angular-maps'`

**Solutions:**
1. **Reinstall package:**
   ```bash
   npm install @syncfusion/ej2-angular-maps --save
   ```
2. **Clear cache:**
   ```bash
   npm cache clean --force
   rm -rf node_modules package-lock.json
   npm install
   ```

### Issue: Feature Not Working (Markers, Tooltips, etc.)

**Symptoms:** Markers/tooltips not appearing despite configuration

**Solutions:**
1. **Inject required service:** Check that you've added the appropriate service to providers
   ```typescript
   providers: [MarkerService, MapsTooltipService]
   ```
2. **Set visible property:** Ensure `visible: true` in feature settings

### Issue: Compatibility Warnings with Older Angular

**Symptoms:** Angular version compatibility warnings

**Solutions:**
- For Angular < 12, use the ngcc package:
  ```bash
  npm install @syncfusion/ej2-angular-maps@ngcc --save
  ```

### Issue: GeoJSON Not Loading

**Symptoms:** Error loading shape data

**Solutions:**
1. **Verify GeoJSON structure:** Ensure it follows proper GeoJSON specification
2. **Check file path:** Verify the import path is correct
3. **Validate JSON:** Use a JSON validator to check for syntax errors
4. **Use TypeScript export:** Export as `object` type, not `any`

### Issue: Performance Issues with Large Maps

**Symptoms:** Slow rendering or laggy interactions

**Solutions:**
1. **Simplify GeoJSON:** Use simplified/lower-resolution shape data
2. **Lazy load features:** Only inject services you need
3. **Optimize data source:** Limit dataSource array size
4. **Use SVG efficiently:** Avoid excessive markers or annotations

## Next Steps

Now that you have a basic map running, explore these topics:

1. **[Layers and Data Binding](layers-and-data.md)** - Learn about multiple layers, sublayers, and data binding patterns
2. **[Markers](markers.md)** - Add location markers with custom shapes and templates
3. **[User Interactions](user-interactions.md)** - Enable zoom, pan, tooltips, and selection
4. **[Data Visualization](data-visualization.md)** - Add bubbles, legends, and color mapping
5. **[Customization](customization.md)** - Theme and style your maps

## Complete Working Example

Here's a complete, copy-paste-ready example:

```typescript
// app.component.ts
import { Component } from '@angular/core';
import { MapsModule, LegendService, DataLabelService, MapsTooltipService } from '@syncfusion/ej2-angular-maps';
import { world_map } from './world-map'; // Your GeoJSON data

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [MapsModule],
  providers: [LegendService, DataLabelService, MapsTooltipService],
  template: `
    <div style="padding: 20px;">
      <h1>World Population Map</h1>
      <ejs-maps 
        id='maps-container'
        [titleSettings]='titleSettings'
        [legendSettings]='legendSettings'>
        <e-layers>
          <e-layer 
            [shapeData]='shapeData'
            [dataSource]='dataSource'
            shapeDataPath='country'
            shapePropertyPath='name'
            [shapeSettings]='shapeSettings'
            [dataLabelSettings]='dataLabelSettings'
            [tooltipSettings]='tooltipSettings'>
          </e-layer>
        </e-layers>
      </ejs-maps>
    </div>
  `,
  styles: [`
    #maps-container {
      height: 500px;
      width: 100%;
    }
  `]
})
export class AppComponent {
  public shapeData: object = world_map;
  
  public dataSource: object[] = [
    { country: 'United States', population: 331002651, density: '36/km²' },
    { country: 'India', population: 1380004385, density: '464/km²' },
    { country: 'China', population: 1439323776, density: '153/km²' },
    { country: 'Brazil', population: 212559417, density: '25/km²' }
  ];
  
  public titleSettings: object = {
    text: 'World Population by Country',
    textStyle: { size: '16px', fontWeight: 'bold' }
  };
  
  public shapeSettings: object = {
    colorValuePath: 'population',
    colorMapping: [
      { from: 0, to: 100000000, color: '#C5E8B7', label: '< 100M' },
      { from: 100000001, to: 500000000, color: '#5BC85A', label: '100M - 500M' },
      { from: 500000001, to: 2000000000, color: '#238B45', label: '> 500M' }
    ]
  };
  
  public legendSettings: object = {
    visible: true,
    position: 'Bottom'
  };
  
  public dataLabelSettings: object = {
    visible: true,
    labelPath: 'country',
    smartLabelMode: 'Trim'
  };
  
  public tooltipSettings: object = {
    visible: true,
    valuePath: 'country',
    format: '${country}<br>Population: ${population}<br>Density: ${density}'
  };
}
```

```css
/* styles.css */
@import '@syncfusion/ej2-base/styles/material.css';
@import '@syncfusion/ej2-angular-maps/styles/material.css';

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 0;
  padding: 0;
}
```

This complete example demonstrates:
- ✅ Installation and setup
- ✅ Module and service injection
- ✅ GeoJSON data binding
- ✅ External data source
- ✅ Color mapping based on population
- ✅ Legend, data labels, and tooltips
- ✅ Title and styling

You now have a fully functional Maps component ready to customize!

---

## API Reference Summary

### Core Setup APIs

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| `MapsComponent` | Main maps component | [MapsComponent](https://ej2.syncfusion.com/angular/documentation/api/maps/mapsComponent) |
| `LayerSettings` | Layer configuration | [LayerSettings](https://ej2.syncfusion.com/angular/documentation/api/maps/layerSettings) |
| `shapeData` | GeoJSON shape data | [shapeData](https://ej2.syncfusion.com/angular/documentation/api/maps/layerSettings#shapedata) |
| `dataSource` | External data binding | [dataSource](https://ej2.syncfusion.com/angular/documentation/api/maps/layerSettings#datasource) |
| `shapeDataPath` | Data matching field | [shapeDataPath](https://ej2.syncfusion.com/angular/documentation/api/maps/layerSettings#shapedatapath) |
| `shapePropertyPath` | GeoJSON matching property | [shapePropertyPath](https://ej2.syncfusion.com/angular/documentation/api/maps/layerSettings#shapepropertypath) |
| `width` | Map width | [width](https://ej2.syncfusion.com/angular/documentation/api/maps/mapsComponent#width) |
| `height` | Map height | [height](https://ej2.syncfusion.com/angular/documentation/api/maps/mapsComponent#height) |
| `theme` | Built-in theme | [theme](https://ej2.syncfusion.com/angular/documentation/api/maps/mapsComponent#theme) |

### Essential Events

| Event | Description | Documentation Link |
|-------|-------------|-------------------|
| `load` | Fires before map loads | [load](https://ej2.syncfusion.com/angular/documentation/api/maps/mapsComponent#load) |
| `loaded` | Fires after map loads | [loaded](https://ej2.syncfusion.com/angular/documentation/api/maps/mapsComponent#loaded) |

**For complete API documentation, see:** [api-reference.md](references/api-reference.md)