---
name: State-persistence
description: 'State persistence in Syncfusion Angular TreeGrid - auto save and restore state, localStorage integration, custom state management, and state events.'
---

# State Persistence

State persistence saves and restores TreeGrid settings like sorting, filtering, and expansion state.

## Table of Contents
- [Enable State Persistence](#enable-state-persistence)
- [Storage Methods](#storage-methods)
- [Custom State Management](#custom-state-management)

## Enable State Persistence

### Basic Persistence

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      id='TreeGrid'
      [dataSource]='data'
      [childMapping]='childMapping'
      [enablePersistence]='true'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='200'></e-column>
      </e-columns>
    </ejs-treegrid>
  `
})
export class AppComponent {
  public data: Object[] = [];
  public childMapping: string = 'subtasks';
}
```

## Storage Methods

### LocalStorage

```typescript
<ejs-treegrid 
  [enablePersistence]='true'>
  <!-- columns -->
</ejs-treegrid>
```

### SessionStorage

// Get in component
```typescript
let value: string = window.localStorage.getItem('treegridTreeGrid'); // treegrid - component name, TreeGrid - component id
let model: Object = JSON.parse(value);
```

// Set in component
```typescript
window.localStorage.setItem('treegridTreeGrid', JSON.stringify(this.getState())); // treegrid - component name, TreeGrid - component id
```

## Custom State Management

### Save State Manually

```typescript
getSavedState() {
  const state: any = {};
  // Save sort settings
  state.sortSettings = this.treegrid.sortSettings;
  // Save filter settings
  state.filterSettings = this.treegrid.filterSettings;
  
  localStorage.setItem('treegridTreeGrid', JSON.stringify(state));
}
```

### Restore State Manually

```typescript
restoreSavedState() {
  const savedState = localStorage.getItem('treegridTreeGrid');
  if (savedState) {
    const state = JSON.parse(savedState);
    this.treegrid.sortSettings = state.sortSettings;
    this.treegrid.filterSettings = state.filterSettings;
  }
}
```
