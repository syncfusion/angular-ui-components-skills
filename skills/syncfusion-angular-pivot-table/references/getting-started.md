# Getting Started with Angular Pivot Grid

## ⚠️ Security Notice

This guide demonstrates using **local data binding**, which is the most secure approach for pivot table implementation. For remote data sources, always refer to the security guidelines in [references/core-concepts.md](./core-concepts.md#security-best-practices).

## Table of Contents
- [Setup Angular Environment](#setup-angular-environment)
- [Create Angular Application](#create-angular-application)
- [Dependencies](#dependencies)
- [Installing Syncfusion PivotView Package](#installing-syncfusion-pivotview-package)
- [Adding CSS Reference](#adding-css-reference)
- [Initializing Pivot Table Component](#initializing-pivot-table-component)
- [Assigning Sample Data](#assigning-sample-data)
- [Adding Fields to Axes](#adding-fields-to-row-column-value-and-filter-axes)

## Setup Angular Environment

Setting up the Angular environment properly ensures smooth development and deployment of your Pivot Table application. Install Angular CLI globally on your system using the following command:

```bash
npm install -g @angular/cli
```

### Installing a Specific Version

To install a particular version of Angular CLI:

```bash
npm install -g @angular/cli@21.0.0
```

## Create Angular Application

Creating a new Angular application provides the foundation for integrating the Syncfusion Angular Pivot Table component. With Angular CLI installed, generate a new project using the command below:

```bash
ng new my-app
```

This command will prompt you for a few settings:
- Stylesheet format (CSS, SCSS, Sass, Less)
- Server-side rendering (SSR) configuration
- AI tool selection

By default, a CSS-based application is created. To use SCSS:

```bash
ng new my-app --style=scss
```

Navigate to the project folder:

```bash
cd my-app
```

## Dependencies

Understanding the dependency structure helps identify the required packages for implementing the Pivot Table component. The Pivot Table component relies on a structured hierarchy of dependencies that provide essential functionality for data processing, user interface elements, and export capabilities.

The following dependency tree shows the required packages for the Angular Pivot Table component:

```javascript
|-- @syncfusion/ej2-angular-pivotview
    |-- @syncfusion/ej2-base
    |-- @syncfusion/ej2-data
    |-- @syncfusion/ej2-pivotview
        |-- @syncfusion/ej2-buttons
        |-- @syncfusion/ej2-dropdowns
        |-- @syncfusion/ej2-excel-export
          |-- @syncfusion/ej2-file-utils
          |-- @syncfusion/ej2-compression
        |-- @syncfusion/ej2-pdf-export
          |-- @syncfusion/ej2-file-utils
          |-- @syncfusion/ej2-compression
        |-- @syncfusion/ej2-grids
        |-- @syncfusion/ej2-inputs
        |-- @syncfusion/ej2-lists
        |-- @syncfusion/ej2-navigations
        |-- @syncfusion/ej2-popups
|-- @syncfusion/ej2-angular-base
```

The main package `@syncfusion/ej2-angular-pivotview` serves as the primary Angular wrapper for the Pivot Table component. This package automatically includes all the necessary sub-dependencies shown in the tree structure above.

## Installing Syncfusion PivotView Package

To build interactive PivotTable in Angular, install the Syncfusion PivotTable package from npm. Syncfusion packages are available as `@syncfusion` scoped packages.

Syncfusion offers two distinct package structures to accommodate different Angular development environments:

### 1. Ivy Library Distribution Package

The Ivy library distribution package is the modern approach to Angular development, designed for Angular 12 and later versions. Syncfusion Angular packages (version 20.2.36 and above) utilize the Ivy distribution format for improved performance and smaller bundle sizes.

```bash
npm install @syncfusion/ej2-angular-pivotview --save
```

### 2. Angular Compatibility Compiler (ngcc) Package

For projects using Angular versions below 12, use the ngcc-compatible package:

```bash
npm install @syncfusion/ej2-angular-pivotview@ngcc --save
```

Update your `package.json` file with the `-ngcc` suffix:

```json
"@syncfusion/ej2-angular-pivotview": "20.2.38-ngcc"
```

> **Note:** Installing without the `-ngcc` suffix will automatically install the Ivy package, which may generate compatibility warnings in Angular versions below 12.

## Adding CSS Reference

Adding the required CSS files ensures that your Angular Pivot Table component displays with proper styling and visual elements. These CSS files contain the necessary styles for all dependent components to render correctly.

Add these CSS imports to your **src/styles.css** file to apply the Material3 theme styling:

```css
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-dropdowns/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-grids/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-inputs/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-lists/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-navigations/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material3.css';
@import "../node_modules/@syncfusion/ej2-splitbuttons/styles/material3.css";
@import '../node_modules/@syncfusion/ej2-angular-pivotview/styles/material3.css';
```

Available theme options: `material3`, `material`, `bootstrap`, `bootstrap5`, `fabric`, `tailwind`, `highcontrast`

## Browser Compatibility

The Pivot Table component provides broad browser compatibility to ensure your application works seamlessly across different environments. For optimal performance in Internet Explorer 11, you will need to include specific polyfills in your Angular application.

## Initializing Pivot Table Component

Setting up the Pivot Table component in your Angular application is straightforward and allows you to create powerful data analysis interfaces with minimal configuration.

Add the following code to your **src/app/app.ts** file:

```typescript
import { PivotViewAllModule, PivotFieldListAllModule } from '@syncfusion/ej2-angular-pivotview'
import { Component, OnInit } from '@angular/core';
import { IDataSet } from '@syncfusion/ej2-angular-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
  imports: [        
    PivotViewAllModule,
    PivotFieldListAllModule
  ],
  standalone: true,
  selector: 'app-root',
  template: `<ejs-pivotview #pivotview id='PivotView' height='350' [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class App implements OnInit {
    public pivotData!: IDataSet[];
    public dataSourceSettings!: DataSourceSettingsModel;

    ngOnInit(): void {
    }
}
```

## Assigning Sample Data

Providing appropriate data to the Pivot Table component enables users to perform meaningful analysis and generate actionable insights from datasets. The Pivot Table component requires a well-structured data source containing the information you want to analyze and visualize.

**🔒 Security Best Practice**: This example uses **local, in-memory data**, which is the most secure approach. Local data eliminates risks associated with remote data fetching, such as indirect prompt injection or data exfiltration.

For demonstration, we'll use a collection of objects containing sales details for various products across different periods and regions:

```typescript
import { PivotViewAllModule, PivotFieldListAllModule } from '@syncfusion/ej2-angular-pivotview'
import { Component, OnInit } from '@angular/core';
import { IDataSet } from '@syncfusion/ej2-angular-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
  imports: [        
    PivotViewAllModule,
    PivotFieldListAllModule
  ],
  standalone: true,
  selector: 'app-root',
  template: `<ejs-pivotview #pivotview id='PivotView' height='350' [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class App implements OnInit {
    public pivotData!: IDataSet[];
    public dataSourceSettings!: DataSourceSettingsModel;

    ngOnInit(): void {
        this.pivotData = [
            { 'Sold': 31, 'Amount': 52824, 'Country': 'France', 'Products': 'Mountain Bikes', 'Year': 'FY 2015', 'Quarter': 'Q1' },
            { 'Sold': 51, 'Amount': 86904, 'Country': 'France', 'Products': 'Mountain Bikes', 'Year': 'FY 2015', 'Quarter': 'Q2' }
        ];

        this.dataSourceSettings = {
            dataSource: this.pivotData
        };
    }
}
```

## Adding Fields to Row, Column, Value and Filter Axes

Organizing fields into appropriate axes transforms raw data into a structured, meaningful Pivot Table. The [`dataSourceSettings`](https://ej2.syncfusion.com/angular/documentation/api/pivotview/datasourcesettings) configuration contains four primary axes:

- **Rows:** Defines the row fields for grouping data vertically
- **Columns:** Defines the column fields for grouping data horizontally
- **Values:** Defines the value fields for data aggregation
- **Filters:** Defines the filter fields for filtering data across both row and column axes

Example configuration:

```typescript
this.dataSourceSettings = {
    dataSource: this.pivotData,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Year' }],
    values: [{ name: 'Amount', type: 'Sum' }],
    filters: []
};
```
