# Getting Started with Angular HeatMap

**API Reference:** [references/api-reference.md](references/api-reference.md)

## Table of Contents

- [Prerequisites](#prerequisites)
- [Package Installation](#package-installation)
  - [Install via npm](#install-via-npm)
  - [Package Versions](#package-versions)
  - [Verify Installation](#verify-installation)
- [Module Setup](#module-setup)
  - [Option 1: Standalone Component (Angular 14+, Recommended)](#option-1-standalone-component-angular-14-recommended)
  - [Option 2: NgModule (Traditional, Backward Compatible)](#option-2-ngmodule-traditional-backward-compatible)
- [Create Your First HeatMap](#create-your-first-heatmap)
  - [Minimal Example](#minimal-example)
  - [In Your HTML Template](#in-your-html-template)
  - [Add Styling](#add-styling)
- [Verify Setup](#verify-setup)
  - [Check if HeatMap Renders](#check-if-heatmap-renders)
  - [Common Setup Issues](#common-setup-issues)

## Prerequisites

Before creating an Angular HeatMap, ensure you have:

- **Node.js & npm:** LTS version installed (use `node --version` to verify)
- **Angular CLI:** Installed globally with `npm install -g @angular/cli`
- **Angular Project:** Created with `ng new my-app` and `cd my-app`
- **Angular Version:** 12+ (recommended 18+)

## Package Installation

### Install via npm

Syncfusion packages are published under the `@syncfusion` scope. Install the HeatMap package with:

```bash
npm install @syncfusion/ej2-angular-heatmap --save
```

### Package Versions

- **Ivy (Recommended):** `@syncfusion/ej2-angular-heatmap` (version 20.2.36+)
  - Supports Angular 21+
  - Modern Ivy rendering engine
  - Better tree-shaking and performance

- **Legacy ngcc:** `@syncfusion/ej2-angular-heatmap@ngcc` (Angular <12)
  - Use only for older Angular versions
  - Compatibility compiler mode

### Verify Installation

Check `package.json` to confirm the package was added:

```json
{
  "dependencies": {
    "@syncfusion/ej2-angular-heatmap": "^32.1.19"
  }
}
```

## Module Setup

### Option 1: Standalone Component (Angular 14+, Recommended)

For modern standalone components, import `HeatMapModule` directly:

```typescript
import { Component, ViewEncapsulation } from '@angular/core';
import { HeatMapModule } from '@syncfusion/ej2-angular-heatmap';

@Component({
    imports: [HeatMapModule],  // Import module here
    standalone: true,
    selector: 'app-heatmap',
    template: `<ejs-heatmap id='heatmap'></ejs-heatmap>`,
    encapsulation: ViewEncapsulation.None
})
export class HeatMapComponent {
}
```

**Advantages:**
- No module file needed
- Tree-shakeable imports
- Cleaner component definition
- Recommended for new projects

### Option 2: NgModule (Traditional, Backward Compatible)

For traditional module-based components, register in `app.module.ts`:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HeatMapModule } from '@syncfusion/ej2-angular-heatmap';
import { AppComponent } from './app.component';

@NgModule({
    declarations: [AppComponent],
    imports: [BrowserModule, HeatMapModule],  // Import module
    providers: [],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

**Use when:**
- Migrating legacy applications
- Organization standardizes on NgModule approach
- Complex feature modules required

## Create Your First HeatMap

### Minimal Example

```typescript
import { Component, ViewEncapsulation } from '@angular/core';
import { HeatMapModule } from '@syncfusion/ej2-angular-heatmap';

@Component({
    imports: [HeatMapModule],
    standalone: true,
    selector: 'my-app',
    template: `
        <ejs-heatmap id='heatmap-container' 
            [dataSource]='dataSource'
            [xAxis]='xAxis'
            [yAxis]='yAxis'
            [cellSettings]='cellSettings'>
        </ejs-heatmap>
    `,
    encapsulation: ViewEncapsulation.None
})
export class AppComponent {
    /**
     * 2D Array Format:
     * Row 1 (Milk):   [2005, 2006, 2007]
     * Row 2 (Bread):  [2005, 2006, 2007]
     * Row 3 (Butter): [2005, 2006, 2007]
     */
    dataSource: number[][] = [
        [21, 22, 23], // Sales for Milk
        [18, 19, 20], // Sales for Bread
        [15, 16, 17]  // Sales for Butter
    ];

    xAxis: Object = {
        labels: ['2005', '2006', '2007'],
        valueType: 'Category'
    };

    yAxis: Object = {
        labels: ['Milk', 'Bread', 'Butter'],
        valueType: 'Category'
    };

    cellSettings: Object = {
        showLabel: true,
        format: '{value}' // Displays the sales number inside the cell
    };
}
```

### In Your HTML Template

If using standalone component in template:

```html
<!-- app.component.html -->
<div class="container">
    <h1>Sales HeatMap</h1>
    <my-ap></my-ap>
</div>
```

### Add Styling

Optional CSS for heatmap container:

```css
/* styles.css */
#heatmap-container {
    width: 100%;
    height: 400px;
    margin-top: 20px;
}
```

## Verify Setup

### Check if HeatMap Renders

1. **Run development server:**
   ```bash
   ng serve
   ```

2. **Open browser:**
   - Navigate to `http://localhost:4200`
   - Look for a grid visualization with colored cells
   - Each cell represents a Sales value
   - Colors indicate value intensity (darker = higher)

3. **Troubleshoot if not rendering:**

   **Problem:** "Cannot find module '@syncfusion/ej2-angular-heatmap'"
   - **Solution:** Run `npm install` again and check package.json

   **Problem:** Module not imported
   - **Solution:** Verify `HeatMapModule` in component's `imports` array

   **Problem:** Component selector not found
   - **Solution:** Ensure selector name (`app-heatmap`) matches in HTML

   **Problem:** No styling applied
   - **Solution:** Add `encapsulation: ViewEncapsulation.None` to component

### Common Setup Issues

**Issue: TypeScript compilation errors**
- Ensure `@angular/core` is also installed: `npm install @angular/core`
- Check Angular version compatibility

**Issue: HeatMap appears but no data**
- Verify `dataSource` array is not empty
- Check `xAxis` and `yAxis` labels match your data

**Issue: Module errors on import**
- Clear node_modules: `rm -r node_modules && npm install`
- Check package version in package.json

