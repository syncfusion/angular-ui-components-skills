# Advanced Tutorials & Patterns

## Table of Contents
- [Overview](#overview)
- [Real-Time Data Updates](#real-time-data-updates)
- [Master-Detail with Filtering](#master-detail-with-filtering)
- [Complex Calculations](#complex-calculations)
- [Advanced Filtering](#advanced-filtering)
- [Custom Themes](#custom-themes)
- [Performance Monitoring](#performance-monitoring)
- [Testing Grids](#testing-grids)

## Overview

This guide covers advanced patterns, real-world scenarios, and best practices for production grids.

---

## Real-Time Data Updates

### Auto-Refresh Grid Data

```typescript
import { Component, ViewChild, OnInit, OnDestroy } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-auto-refresh-grid',
  template: `
    <div>
      <label>Refresh every:
        <select [(ngModel)]="refreshInterval" (change)="onRefreshIntervalChange()">
          <option [value]="1000">1 second</option>
          <option [value]="5000">5 seconds</option>
          <option [value]="10000">10 seconds</option>
          <option [value]="30000">30 seconds</option>
        </select>
      </label>

      <ejs-grid #grid [dataSource]="data" [allowPaging]="true">
        <e-columns>
          <!-- columns -->
        </e-columns>
      </ejs-grid>
    </div>
  `
})
export class AutoRefreshGridComponent implements OnInit, OnDestroy {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  refreshInterval = 5000;
  private intervalId: any;

  ngOnInit() {
    this.startAutoRefresh();
  }

  ngOnDestroy() {
    if (this.intervalId) {
      clearInterval(this.intervalId);
    }
  }

  startAutoRefresh() {
    this.intervalId = setInterval(async () => {
      try {
        const response = await fetch('/api/orders');
        this.data = await response.json();
      } catch (error) {
        console.error('Refresh error:', error);
      }
    }, this.refreshInterval);
  }

  onRefreshIntervalChange() {
    if (this.intervalId) {
      clearInterval(this.intervalId);
    }
    this.startAutoRefresh();
  }
}
```

### WebSocket Real-Time Updates

```typescript
import { Component, ViewChild, OnInit, OnDestroy } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-websocket-grid',
  template: `
    <ejs-grid #grid [dataSource]="data" [allowPaging]="true">
      <e-columns>
        <!-- columns -->
      </e-columns>
    </ejs-grid>
  `
})
export class WebSocketGridComponent implements OnInit, OnDestroy {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  private ws: WebSocket;

  ngOnInit() {
    this.connectWebSocket();
  }

  ngOnDestroy() {
    if (this.ws) {
      this.ws.close();
    }
  }

  connectWebSocket() {
    this.ws = new WebSocket('ws://api.example.com/orders');

    this.ws.onmessage = (event) => {
      const newData = JSON.parse(event.data);
      const index = this.data.findIndex(item => item.OrderID === newData.OrderID);
      
      if (index >= 0) {
        this.data[index] = newData;
      } else {
        this.data.push(newData);
      }
      
      this.gridInstance.refresh();
    };

    this.ws.onerror = (error) => {
      console.error('WebSocket error:', error);
    };
  }
}
```

### SignalR Real-Time Updates

```typescript
import { Component, ViewChild, OnInit, OnDestroy } from '@angular/core';
import * as signalR from '@microsoft/signalr';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-signalr-grid',
  template: `
    <ejs-grid #grid [dataSource]="data" [allowPaging]="true">
      <e-columns>
        <!-- columns -->
      </e-columns>
    </ejs-grid>
  `
})
export class SignalRGridComponent implements OnInit, OnDestroy {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  private connection: signalR.HubConnection;

  ngOnInit() {
    this.initializeSignalR();
  }

  ngOnDestroy() {
    if (this.connection) {
      this.connection.stop();
    }
  }

  initializeSignalR() {
    this.connection = new signalR.HubConnectionBuilder()
      .withUrl('/orderHub')
      .withAutomaticReconnect()
      .build();

    this.connection.on('OrderUpdated', (updatedOrder) => {
      const index = this.data.findIndex(o => o.OrderID === updatedOrder.OrderID);
      if (index >= 0) {
        this.data[index] = updatedOrder;
        this.gridInstance.setCellValue(index, 'Freight', updatedOrder.Freight);
      }
    });

    this.connection.on('OrderAdded', (newOrder) => {
      this.gridInstance.addRecord(newOrder);
    });

    this.connection.start().catch(err => console.error('Connection error:', err));
  }
}
```

---

## Master-Detail with Filtering

### Filtered Detail Grid

```typescript
import { Component, OnInit } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-master-detail-grid',
  template: `
    <ejs-grid [dataSource]="orders" (recordClick)="handleMasterRowClick($event)">
      <ng-template #detailTemplate let-data>
        <ejs-grid [dataSource]="getOrderDetails(data.OrderID)" [allowPaging]="true" [pageSettings]="{ pageSize: 5 }">
          <e-columns>
            <e-column field="ProductID" headerText="Product ID" width="100"></e-column>
            <e-column field="ProductName" headerText="Product" width="150"></e-column>
            <e-column field="Quantity" headerText="Quantity" width="100"></e-column>
            <e-column field="UnitPrice" headerText="Unit Price" width="100" format="C2"></e-column>
          </e-columns>
        </ejs-grid>
      </ng-template>
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerID" headerText="Customer" width="120"></e-column>
        <e-column field="OrderDate" headerText="Order Date" type="date" format="yMd"></e-column>
        <e-column field="Freight" headerText="Freight" width="100" format="C2"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class MasterDetailGridComponent implements OnInit {
  orders: any[] = [];
  selectedOrder: any;

  ngOnInit() {
    this.loadOrders();
  }

  loadOrders() {
    // Load orders data
    this.orders = [...]; // Load from service
  }

  getOrderDetails(orderId: number) {
    // Fetch and return detail data for the order
    return [];
  }

  handleMasterRowClick(args: any) {
    this.selectedOrder = args.rowData;
  }
}
```

---

## Complex Calculations

### Calculated Columns with Aggregates

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-calculations-grid',
  template: `
    <ejs-grid [dataSource]="data">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="Freight" headerText="Freight" width="100" format="C2"></e-column>
        <e-column field="Tax" headerText="Tax (10%)" width="100" format="C2"></e-column>
        <e-column field="Total" headerText="Total" width="100" format="C2"></e-column>
      </e-columns>
      <e-aggregates>
        <e-aggregate>
          <e-columns>
            <e-column field="Freight" type="Sum"></e-column>
            <e-column field="Tax" type="Sum"></e-column>
            <e-column field="Total" type="Sum"></e-column>
          </e-columns>
        </e-aggregate>
      </e-aggregates>
    </ejs-grid>
  `
})
export class CalculationsGridComponent implements OnInit {
  data: any[] = [];

  ngOnInit() {
    this.loadOrdersWithCalculations();
  }

  loadOrdersWithCalculations() {
    const orders = [...]; // Load from service
    this.data = orders.map(order => ({
      ...order,
      Tax: order.Freight * 0.1,
      Total: order.Freight + (order.Freight * 0.1)
    }));
  }
}
```

### Running Totals

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-running-total-grid',
  template: `
    <ejs-grid [dataSource]="enhancedData">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="Freight" headerText="Freight" width="100" format="C2"></e-column>
        <e-column field="RunningTotal" headerText="Running Total" width="150" format="C2"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class RunningTotalGridComponent implements OnInit {
  enhancedData: any[] = [];
  private runningTotal = 0;

  ngOnInit() {
    this.calculateRunningTotals();
  }

  calculateRunningTotals() {
    const orders = [...]; // Load from service
    this.runningTotal = 0;
    this.enhancedData = orders.map(order => {
      this.runningTotal += order.Freight;
      return {
        ...order,
        RunningTotal: this.runningTotal
      };
    });
  }
}
```

---

## Advanced Filtering

### Multi-Column Complex Filter

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';
import { Predicate, Query } from '@syncfusion/ej2-data';
import { FilterService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-complex-filter-grid',
  template: `
    <button (click)="applyAdvancedFilter()">Apply Complex Filter</button>

    <ejs-grid #grid [dataSource]="orders" [allowFiltering]="true" [filterSettings]="{ type: 'Menu' }">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerID" headerText="Customer" width="120"></e-column>
        <e-column field="Freight" headerText="Freight" width="100" format="C2"></e-column>
        <e-column field="Status" headerText="Status" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [FilterService]
})
export class ComplexFilterGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  orders: any[] = [];

  ngOnInit() {
    this.loadOrders();
  }

  loadOrders() {
    this.orders = [...]; // Load from service
  }

  applyAdvancedFilter() {
    const predicate = new Predicate('Freight', 'greaterThan', 50);
    predicate.and('Status', 'equal', 'Active');
    predicate.or('CustomerID', 'equal', 'VINET');

    this.gridInstance.query = new Query().where(predicate);
    this.gridInstance.refresh();
  }
}
```

### Date Range Filtering

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';
import { Predicate, Query } from '@syncfusion/ej2-data';
import { FilterService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-date-range-filter-grid',
  template: `
    <div>
      <label>Start Date:
        <input type="date" [(ngModel)]="startDateString" (change)="onStartDateChange()"</e-column>
      </label>
      <label>End Date:
        <input type="date" [(ngModel)]="endDateString" (change)="onEndDateChange()"</e-column>
      </label>
      <button (click)="applyDateFilter()">Apply Date Filter</button>
    </div>

    <ejs-grid #grid [dataSource]="orders" [allowFiltering]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="OrderDate" headerText="Order Date" type="date" format="yMd" width="120"></e-column>
        <e-column field="Freight" headerText="Freight" width="100" format="C2"></e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [FilterService]
})
export class DateRangeFilterGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  orders: any[] = [];
  startDate: Date;
  endDate: Date;
  startDateString: string;
  endDateString: string;

  ngOnInit() {
    this.loadOrders();
  }

  loadOrders() {
    this.orders = [...]; // Load from service
  }

  onStartDateChange() {
    this.startDate = new Date(this.startDateString);
  }

  onEndDateChange() {
    this.endDate = new Date(this.endDateString);
  }

  applyDateFilter() {
    if (this.startDate && this.endDate) {
      const predicate = new Predicate('OrderDate', 'greaterthanorequal', this.startDate);
      predicate.and('OrderDate', 'lessthanorequal', this.endDate);

      this.gridInstance.query = new Query().where(predicate);
      this.gridInstance.refresh();
    }
  }
}
```

---

## Custom Themes

### Create Custom Theme

```typescript
import { Component, OnInit } from '@angular/core';\nimport { DomSanitizer, SafeHtml } from '@angular/platform-browser';\n\n@Component({\n  selector: 'app-custom-theme-grid',\n  template: `\n    <link [href]=\"themeUrl\" rel=\"stylesheet\"</e-column>\n    <ejs-grid [dataSource]=\"data\">\n      <e-columns>\n        <!-- columns -->\n      </e-columns>\n    </ejs-grid>\n  `,\n  styles: [`\n    .e-grid .e-headercell {\n      background-color: #2c3e50;\n      color: #ecf0f1;\n      font-weight: 600;\n      padding: 12px;\n    }\n    \n    .e-grid .e-row {\n      border-bottom: 1px solid #ecf0f1;\n    }\n    \n    .e-grid .e-row:hover {\n      background-color: #f8f9fa;\n    }\n    \n    .e-grid .e-selectionbackground {\n      background-color: #3498db;\n      color: white;\n    }\n    \n    .e-grid .e-row:nth-child(even) {\n      background-color: #f8f9fa;\n    }\n    \n    .e-grid .e-gridcontent td {\n      padding: 12px;\n      color: #2c3e50;\n    }\n  `]\n})\nexport class CustomThemeGridComponent implements OnInit {\n  data: any[] = [];\n  themeUrl: string;\n\n  constructor(private sanitizer: DomSanitizer) {}\n\n  ngOnInit() {\n    this.loadData();\n  }\n\n  loadData() {\n    this.data = [...]; // Load from service\n  }\n}
```

### Dynamic Theme Switching

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-theme-switcher-grid',
  template: `
    <select [(ngModel)]="theme" (change)="applyTheme()">
      <option value="light">Light</option>
      <option value="dark">Dark</option>
      <option value="blue">Blue</option>
    </select>

    <div [ngClass]="'themed-grid-' + theme">
      <ejs-grid [dataSource]="data">
        <e-columns>
          <!-- columns -->
        </e-columns>
      </ejs-grid>
    </div>
  `,
  styles: [`
    .themed-grid-light {
      background-color: #ffffff;
      color: #000000;
    }
    
    .themed-grid-light .e-headercell {
      background-color: #f5f5f5;
      color: #000000;
    }
    
    .themed-grid-dark {
      background-color: #1e1e1e;
      color: #ffffff;
    }
    
    .themed-grid-dark .e-headercell {
      background-color: #333333;
      color: #ffffff;
    }
    
    .themed-grid-blue {
      background-color: #e3f2fd;
      color: #1565c0;
    }
    
    .themed-grid-blue .e-headercell {
      background-color: #1976d2;
      color: #ffffff;
    }
  `]
})
export class ThemeSwitcherGridComponent implements OnInit {
  data: any[] = [];
  theme = 'light';

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    this.data = [...]; // Load from service
  }

  applyTheme() {
    // Theme applied via ngClass binding
  }
}
```

---

## Performance Monitoring

### Measure Grid Performance

```typescript
import { Component, ViewChild, OnInit, OnDestroy } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-performance-monitor-grid',
  template: `
    <button (click)="measureInitialLoad()">Measure Load Time</button>
    <button (click)="countDataFetches()">Show Network Metrics</button>

    <ejs-grid #grid [dataSource]="data" (actionComplete)="onActionComplete($event)">
      <e-columns>
        <!-- columns -->
      </e-columns>
    </ejs-grid>
  `
})
export class PerformanceGridComponent implements OnInit, OnDestroy {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  private observer: PerformanceObserver;

  ngOnInit() {
    this.initializePerformanceMonitoring();
  }

  ngOnDestroy() {
    if (this.observer) {
      this.observer.disconnect();
    }
  }

  initializePerformanceMonitoring() {
    this.observer = new PerformanceObserver((list) => {
      for (const entry of list.getEntries()) {
        console.log(`${entry.name}: ${entry.duration}ms`);
      }
    });

    this.observer.observe({ entryTypes: ['measure'] });
  }

  measureInitialLoad() {
    performance.mark('grid-load-start');
    
    // Grid renders here
    
    performance.mark('grid-load-end');
    performance.measure('grid-load', 'grid-load-start', 'grid-load-end');
  }

  countDataFetches() {
    const navigationTiming = performance.getEntriesByType('navigation')[0];
    console.log('Network timing:', {
      dnsLookup: navigationTiming.domainLookupEnd - navigationTiming.domainLookupStart,
      tcpConnect: navigationTiming.connectEnd - navigationTiming.connectStart,
      ttfb: navigationTiming.responseStart - navigationTiming.requestStart,
      download: navigationTiming.responseEnd - navigationTiming.responseStart
    });
  }

  onActionComplete(args: any) {
    console.log(`Action '${args.requestType}' completed at ${performance.now()}ms`);
  }
}
```

---


## Testing Grids

### Unit Tests

```typescript
import { TestBed, ComponentFixture } from '@angular/core/testing';
import { GridComponent } from '@syncfusion/ej2-angular-grids';
import { PerformanceGridComponent } from './performance-grid.component';

describe('Grid Component Tests', () => {
  let component: PerformanceGridComponent;
  let fixture: ComponentFixture<PerformanceGridComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [PerformanceGridComponent, GridComponent]
    }).compileComponents();

    fixture = TestBed.createComponent(PerformanceGridComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should create grid component', () => {
    expect(component).toBeTruthy();
  });

  it('should render grid with data', () => {
    component.data = mockData;
    fixture.detectChanges();
    const gridElement = fixture.nativeElement.querySelector('.e-grid');
    expect(gridElement).toBeTruthy();
  });

  it('should display correct number of rows', (done) => {
    component.data = mockData;
    fixture.detectChanges();
    
    setTimeout(() => {
      const rows = fixture.nativeElement.querySelectorAll('.e-row');
      expect(rows.length).toBe(mockData.length);
      done();
    }, 100);
  });

  it('should handle sorting', (done) => {
    component.data = mockData;
    fixture.detectChanges();
    
    const headerCell = fixture.nativeElement.querySelector('[data-field="OrderID"]');
    headerCell.click();
    fixture.detectChanges();
    
    setTimeout(() => {
      const firstRow = fixture.nativeElement.querySelector('.e-row');
      expect(firstRow.textContent).toContain('10248');
      done();
    }, 100);
  });

  it('should handle filtering', (done) => {
    component.data = mockData;
    fixture.detectChanges();
    
    component.gridInstance.filterByColumn('OrderID', 'equal', 10248);
    fixture.detectChanges();
    
    setTimeout(() => {
      const rows = fixture.nativeElement.querySelectorAll('.e-row');
      expect(rows.length).toBe(1);
      done();
    }, 100);
  });

  it('should handle inline editing', (done) => {
    component.data = mockData;
    component.gridInstance.allowInlineEdit = true;
    fixture.detectChanges();
    
    component.gridInstance.startEdit(0);
    fixture.detectChanges();
    
    setTimeout(() => {
      const editCell = fixture.nativeElement.querySelector('.e-editedinput');
      expect(editCell).toBeTruthy();
      done();
    }, 100);
  });
});

const mockData = [
  { OrderID: 10248, CustomerID: 'VINET', OrderDate: new Date(1996, 6, 4), Freight: 32.38 },
  { OrderID: 10249, CustomerID: 'TOMSP', OrderDate: new Date(1996, 6, 5), Freight: 11.61 }
];
    const fixture = TestBed.createComponent(GridComponent);
    const component = fixture.componentInstance;
    component.mockData = mockData;
    fixture.detectChanges();
    
    const filterInput = fixture.nativeElement.querySelector('.e-filterinput');
    filterInput.value = 'VINET';
    filterInput.dispatchEvent(new Event('change'));
    fixture.detectChanges();
    
    const rows = fixture.nativeElement.querySelectorAll('.e-row');
    expect(rows.length).toBeLessThan(mockData.length);
  });

  test('editing saves data correctly', () => {
    const fixture = TestBed.createComponent(GridComponent);
    const component = fixture.componentInstance;
    component.mockData = mockData;
    component.editSettings = { mode: 'Inline' };
    fixture.detectChanges();
    
    const editButton = fixture.nativeElement.querySelector('[aria-label="Edit"]');
    editButton.click();
    fixture.detectChanges();
    
    const input = fixture.nativeElement.querySelector('input');
    input.value = 'NewValue';
    input.dispatchEvent(new Event('change'));
    
    const saveButton = fixture.nativeElement.querySelector('[aria-label="Save"]');
    saveButton.click();
    fixture.detectChanges();
    
    expect(mockData[0].CustomerID).toBe('NewValue');
  });
});

import { TestBed, ComponentFixture } from '@angular/core/testing';
```

### Integration Tests

```typescript
describe('Grid Integration Tests', () => {
  test('pagination, sorting, and filtering work together', () => {
    const fixture = TestBed.createComponent(GridComponent);
    const component = fixture.componentInstance;
    component.largeDataset = largeDataset;
    component.allowPaging = true;
    component.allowSorting = true;
    component.allowFiltering = true;
    fixture.detectChanges();

    // Apply filter
    const filterInput = fixture.nativeElement.querySelector('.e-filterinput');
    filterInput.value = 'Berlin';
    filterInput.dispatchEvent(new Event('change'));
    fixture.detectChanges();

    // Sort
    const sortHeader = fixture.nativeElement.querySelector('[data-field="OrderDate"]');
    sortHeader.click();
    fixture.detectChanges();

    // Change page
    const pageInput = fixture.nativeElement.querySelector('.e-pagejump input');
    pageInput.value = '2';
    pageInput.dispatchEvent(new Event('change'));
    fixture.detectChanges();

    const rows = fixture.nativeElement.querySelectorAll('.e-row');
    expect(rows.length).toBeGreaterThan(0);
  });

  test('edit and save with validation', () => {
    const onSave = jasmine.createSpy('onSave');
    const fixture = TestBed.createComponent(GridComponent);
    const component = fixture.componentInstance;
    component.mockData = mockData;
    component.editSettings = { mode: 'Dialog', allowEditing: true };
    component.actionComplete = onSave;
    fixture.detectChanges();

    // Start edit
    const editButton = fixture.nativeElement.querySelector('[aria-label="Edit"]');
    editButton.click();
    fixture.detectChanges();

    // Fill form
    const form = fixture.nativeElement.querySelector('.e-dialog-component');
    const inputs = form.querySelectorAll('input');
    inputs[0].value = 'UpdatedValue';
    inputs[0].dispatchEvent(new Event('change'));
    fixture.detectChanges();

    // Save
    const saveButton = form.querySelector('.e-primary');
    saveButton.click();
    fixture.detectChanges();

    expect(onSave).toHaveBeenCalled();
  });
});
```

---

## Best Practices Summary

1. **Real-time data**: Use WebSocket or SignalR for live updates
2. **Master-detail**: Filter child data based on parent selection
3. **Calculations**: Pre-compute complex values before binding
4. **Filtering**: Use Predicate for complex queries
5. **Themes**: Create reusable theme CSS modules
6. **Performance**: Monitor with Performance API
7. **Testing**: Test user interactions and data scenarios
8. **Error handling**: Gracefully handle API failures
9. **Accessibility**: Ensure keyboard navigation works
10. **Documentation**: Document custom configurations

