# Getting Started with Syncfusion Angular Block Editor

## Table of Contents
- [Installation](#installation)
- [Dependencies](#dependencies)
- [Angular Environment Setup](#angular-environment-setup)
- [Creating an Angular Application](#creating-an-angular-application)
- [Installing the Package](#installing-the-package)
- [Adding CSS Reference](#adding-css-reference)
- [Creating Your First Editor](#creating-your-first-editor)
- [Running the Application](#running-the-application)

## Installation

The Syncfusion Angular Block Editor component is distributed as npm packages under the `@syncfusion` scope. Start by installing the package for your Angular version.

### Prerequisites
- Node.js and npm installed
- Angular CLI installed globally
- Familiarity with Angular project structure

## Dependencies

The Block Editor requires the following dependencies:

```
@syncfusion/ej2-angular-blockeditor
  ├── @syncfusion/ej2-angular-base
  ├── @syncfusion/ej2-base
  ├── @syncfusion/ej2-popups
  ├── @syncfusion/ej2-buttons
  ├── @syncfusion/ej2-splitbuttons
  ├── @syncfusion/ej2-navigations
  ├── @syncfusion/ej2-dropdowns
  └── @syncfusion/ej2-inputs
```

These are automatically installed as peer dependencies when you install the main package.

## Angular Environment Setup
skills\documentations\syncfusion-angular-blockeditor\references
### Step 1: Install Angular CLI

If you don't have the Angular CLI installed globally, run:

```bash
npm install -g @angular/cli
```

### Step 2: Create a New Angular Project

```bash
ng new my-block-editor-app
cd my-block-editor-app
```

Choose your preferences for routing and stylesheet format when prompted.

## Creating an Angular Application

Create a new Angular project or use an existing one:

```bash
ng new my-app
cd my-app
```

## Installing the Package

The latest Syncfusion packages use Ivy distribution and are compatible with Angular 12 and newer.

```bash
npm install @syncfusion/ej2-angular-blockeditor --save
```

## Adding CSS Reference

Import the required CSS files in your `src/styles.css`:

```css
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-inputs/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-splitbuttons/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-navigations/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-dropdowns/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-angular-blockeditor/styles/material3.css';
```

You can also use alternative themes like `tailwind3.css` or `bootstrap5.css` depending on your preference.

## Creating Your First Editor

### Step 1: Import the Module

In your `app.component.ts`:

```typescript
import { Component } from '@angular/core';
import { BlockEditorModule } from '@syncfusion/ej2-angular-blockeditor';

@Component({
    imports: [BlockEditorModule],
    standalone: true,
    selector: 'app-root',
    template: `<div id="container">
        <ejs-blockeditor />
    </div>`
})
export class AppComponent { }
```

### Step 2: Configure in Template

Update your component template to include the Block Editor:

```typescript
import { Component } from '@angular/core';
import { BlockEditorModule } from '@syncfusion/ej2-angular-blockeditor';
import { BlockModel, ContentType } from '@syncfusion/ej2-blockeditor';

@Component({
    imports: [BlockEditorModule],
    standalone: true,
    selector: 'app-root',
    template: `<div id="container">
        <ejs-blockeditor [blocks]="blocksData" />
    </div>`
})
export class AppComponent {
    public blocksData: BlockModel[] = [
        {
            blockType: 'Heading',
            properties: { level: 1 },
            content: [
                {
                    contentType: ContentType.Text,
                    content: 'Welcome to Block Editor'
                }
            ]
        },
        {
            blockType: 'Paragraph',
            content: [
                {
                    contentType: ContentType.Text,
                    content: 'Start editing your content here...'
                }
            ]
        }
    ];
}
```

## Running the Application

Start your Angular development server:

```bash
ng serve
```

Navigate to `http://localhost:4200/` in your browser. You should see the Block Editor component ready for use.

### Verify Installation

- Check that the editor renders without errors
- Verify you can type in the editor
- Try using "/" to open the slash command menu
- Confirm CSS styles are applied correctly

If you see styling issues, double-check that all CSS imports are present in your `styles.css` file and that the paths are correct for your Syncfusion version.

