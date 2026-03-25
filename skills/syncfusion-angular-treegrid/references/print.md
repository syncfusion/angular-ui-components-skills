---
name: Print
description: 'Printing in Syncfusion Angular TreeGrid - print functionality, print modes, page setup control, custom print templates, and print events.'
---

# Printing

Printing allows users to generate print-friendly representations of TreeGrid data.

## Table of Contents
- [Enable Printing](#enable-printing)
- [Print Modes](#print-modes)
- [Print Events](#print-events)

## Enable Printing

### Basic Print

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';
import { ToolbarService } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      [childMapping]='childMapping'
      [toolbar]='toolbarItems'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='200'></e-column>
      </e-columns>
    </ejs-treegrid>
  `,
  providers: [ToolbarService]
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  public data: Object[] = [];
  public childMapping: string = 'subtasks';
  public toolbarItems = ['Print'];
}
```
