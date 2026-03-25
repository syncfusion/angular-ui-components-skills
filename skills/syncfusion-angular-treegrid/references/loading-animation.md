---
name: Loading-animation
description: 'Loading animation in Syncfusion Angular TreeGrid - spinner display configuration, custom loader templates, animation events, and loading state management.'
---

# Loading Animation

Loading animation displays a spinner while data is being loaded or processed.

## Table of Contents
- [Display Loading](#display-loading)
- [Spinner Configuration](#spinner-configuration)
- [Custom Templates](#custom-templates)

## Display Loading

### Show Spinner

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <button (click)='showLoading()'>Load Data</button>
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      [childMapping]='childMapping'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='200'></e-column>
      </e-columns>
    </ejs-treegrid>
  `
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  public data: Object[] = [];
  public childMapping: string = 'subtasks';

  showLoading() {
    this.treegrid.showSpinner();
    setTimeout(() => {
      this.treegrid.hideSpinner();
    }, 2000);
  }
}
```