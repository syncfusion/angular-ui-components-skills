# Getting Started

## Table of Contents
- [When to Use This Skill](#when-to-use-this-skill)
- [Installation](#installation)
- [Package Setup](#package-setup)
- [CSS Imports](#css-imports)
- [Basic Grid Initialization](#basic-grid-initialization)
- [Minimal Working Example](#minimal-working-example)

## When to Use This Skill

Use this skill when you need to:
- **Set up Angular Grid** — Install and configure Syncfusion Grid in Angular projects
- **Install packages** — Add required npm packages
- **Configure modules** — Import GridModule in Angular modules
- **Add CSS styles** — Include required Syncfusion stylesheets
- **Initialize grid** — Create first working grid component
- **Configure data binding** — Show how to bind data to grid
- **Basic column setup** — Define and configure columns
- **CSS troubleshooting** — Fix unstyled pager and other components

## Installation

Install Syncfusion Angular Grid component via npm:

```bash
npm install @syncfusion/ej2-angular-grids
```

## Package Setup

Import the GridModule in your Angular module:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { GridModule } from '@syncfusion/ej2-angular-grids';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, GridModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## CSS Imports

> **CSS Quick Reference:** Always import **all** required Syncfusion CSS files in your `App.css` (or equivalent). Each feature depends on a specific package's CSS:
> - **Pager** (page number buttons, navigation) → `@syncfusion/ej2-angular-grids/styles/material.css` — **missing this is the most common cause of unstyled pagers**
> - **Toolbar** → `@syncfusion/ej2-navigations/styles/material.css`
> - **Filter/Edit inputs** → `@syncfusion/ej2-inputs/styles/material.css`
> - **Dialog editing, filter menus** → `@syncfusion/ej2-popups/styles/material.css`

Include required CSS files in your main styles or component:

```typescript
// In styles.css or component.css
@import '@syncfusion/ej2-base/styles/material.css';
@import '@syncfusion/ej2-grids/styles/material.css';
```

**Available themes:**
- material (default)
- material-dark
- fabric
- fabric-dark
- bootstrap
- bootstrap-dark
- bootstrap5
- tailwind
- fluent
- fluent-dark

## Basic Grid Initialization

Create a grid with minimal configuration:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-grid',
  template: `
    <ejs-grid [dataSource]="data">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="TotalAmount" headerText="Total Amount" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridComponent {
  data = [
    { OrderID: 10248, CustomerName: 'VINET', TotalAmount: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', TotalAmount: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', TotalAmount: 65.83 }
  ];
}
```

## Minimal Working Example

A complete, copy-paste-ready example with basic features:

**grid.component.ts:**
```typescript
import { Component, OnInit } from '@angular/core';

interface Order {
  OrderID: number;
  CustomerName: string;
  ShipAddress: string;
  TotalAmount: number;
}

@Component({
  selector: 'app-grid',
  templateUrl: './grid.component.html',
  styleUrls: ['./grid.component.css']
})
export class GridComponent implements OnInit {
  data: Order[] = [];

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    this.data = [
      { OrderID: 10248, CustomerName: 'VINET', ShipAddress: '59 rue de l\'Abbaye', TotalAmount: 32.38 },
      { OrderID: 10249, CustomerName: 'TOMSP', ShipAddress: 'Luisenstr. 48', TotalAmount: 11.61 },
      { OrderID: 10250, CustomerName: 'HANAR', ShipAddress: 'Rua do Paço, 67', TotalAmount: 65.83 },
      { OrderID: 10251, CustomerName: 'VICTE', ShipAddress: '2, rue du Commerce', TotalAmount: 41.34 },
      { OrderID: 10252, CustomerName: 'SUPRD', ShipAddress: 'Boulevard Tirou, 255', TotalAmount: 51.30 }
    ];
  }
}
```

**grid.component.html:**
```html
<ejs-grid [dataSource]="data" [allowPaging]="true" [pageSettings]="{ pageSize: 10 }">
  <e-columns>
    <e-column field="OrderID" headerText="Order ID" type="number" width="100"></e-column>
    <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
    <e-column field="ShipAddress" headerText="Ship Address" width="200"></e-column>
    <e-column field="TotalAmount" headerText="Total Amount" type="number" format="C2" width="120"></e-column>
  </e-columns>
</ejs-grid>
```

**grid.component.css:**
```css
:host ::ng-deep .e-grid {
  font-family: Arial, sans-serif;
}

:host ::ng-deep .e-gridheader {
  background-color: #f5f5f5;
}
```
