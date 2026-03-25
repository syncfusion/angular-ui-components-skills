# Validation in Angular Grid

## Table of Contents
- [Overview](#overview)
- [Built-in Validators](#built-in-validators)
- [Validation Rule Definition](#validation-rule-definition)
- [Custom Validation](#custom-validation)
- [Async Validation](#async-validation)
- [Error Display](#error-display)
- [Validation Events](#validation-events)
- [Server-Side Validation](#server-side-validation)

## Overview

Validation ensures data quality and consistency before saving to the database. Syncfusion Grid supports:
- **Built-in validators** (required, min, max, pattern)
- **Custom validation functions**
- **Async validation** (server-side checks)
- **Error messages and styling**
- **Validation events** for hooks

---

## Built-in Validators

### Required Field Validation

```typescript
import { Component } from '@angular/core';
import { EditService, ToolbarService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid',
  template: `
    <ejs-grid [dataSource]="data"
              [editSettings]="{ mode: 'Dialog', allowEditing: true }"
              toolbar="['Edit', 'Update', 'Cancel']">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100" 
                  [isPrimaryKey]="true" 
                  [validationRules]="{required: true}"></e-column>
        <e-column field="CustomerID" headerText="Customer ID" width="120" 
                  [validationRules]="{required: true}"></e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [EditService, ToolbarService]
})
export class GridComponent {
  data = [];
}
```

### Number Range Validation

```typescript
<e-column field="Freight" headerText="Freight" width="100" format="C2"
          [validationRules]="{ required: true, min: 0, max: 1000 }"></e-column>
```

### Date Validation

```typescript
<e-column
  field="OrderDate"
  headerText="Order Date"
  type="date"
  format="yMd"
  [editParams]="dateEditParams">
</e-column>

// In component:
dateEditParams = {
  params: {
    required: true,
    min: new Date(2020, 0, 1),
    max: new Date()
  }
};
```

### Pattern/Regex Validation

```typescript
<e-column
  field="Email"
  headerText="Email"
  [editParams]="emailEditParams">
</e-column>

// In component:
emailEditParams = {
  params: {
    required: true,
    pattern: /[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,4}/
  }
};
```

---

## Validation Rule Definition

### Define Validation Rules on Column

```typescript
const orderIDRules = {
  required: { message: 'Order ID is required' },
  min: { value: 1, message: 'Order ID must be at least 1' }
};

const freightRules = {
  required: { message: 'Freight is required' },
  min: { value: 0, message: 'Freight cannot be negative' },
  max: { value: 5000, message: 'Freight cannot exceed 5000' }
};

import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-validation-rules-grid',
  template: `<ejs-grid #grid [dataSource]="data">
    <e-columns>
      <e-column
        field="OrderID"
        headerText="Order ID"
        [editParams]="orderIDEditParams"></e-column>
      <e-column
        field="Freight"
        headerText="Freight"
        [editParams]="freightEditParams"></e-column>
    </e-columns>
  </ejs-grid>`,
  providers: [EditService]
})
export class ValidationRulesGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  
  orderIDEditParams = { params: { required: true, min: { value: 1, message: 'Order ID must be at least 1' } } };
  freightEditParams = { params: { required: true, min: { value: 0, message: 'Freight cannot be negative' }, max: { value: 5000, message: 'Freight cannot exceed 5000' } } };

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }
}
```

### Validation Object Structure

```typescript
const validationRules = {
  required: true | { message: 'Custom message' },
  min: number | { value: number, message: 'Custom message' },
  max: number | { value: number, message: 'Custom message' },
  minLength: number | { value: number, message: 'Custom message' },
  maxLength: number | { value: number, message: 'Custom message' },
  pattern: RegExp | { pattern: RegExp, message: 'Custom message' },
  custom: validationFunction
};
```

---

## Custom Validation

### Simple Custom Validation

```typescript
import { Component } from '@angular/core';
import { EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-custom-validator',
  template: `
    <ejs-grid [dataSource]="data">
      <e-columns>
        <e-column field="Email" headerText="Email"
                  [validationRules]="{ required: true }"></e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [EditService]
})
export class CustomValidatorComponent {
  data: any[] = [];

  validateEmail(value: string): boolean {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return regex.test(value);
  }
}
```

### Custom Validation with Context

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent, EditService, ToolbarService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-validation-context-grid',
  template: `<ejs-grid #grid
    [dataSource]="data"
    [editSettings]="editSettings"
    (actionBegin)="onActionBegin($event)">
    <!-- columns -->
  </ejs-grid>`,
  providers: [EditService, ToolbarService]
})
export class ValidationContextGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  editSettings = { mode: 'Dialog' };

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }

  validateFreightMinimum(args: any) {
    if (args.data && args.data.Freight !== undefined) {
      if (args.data.Freight < 10) {
        args.cancel = true;
        this.showError('Freight must be at least $10');
      }
    }
  }

  onActionBegin(args: any) {
    if (args.requestType === 'save') {
      this.validateFreightMinimum(args);
    }
  }

  showError(message: string) {
    console.error(message);
  }
}
```

### Multi-Field Validation

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-multifield-validation-grid',
  template: `<ejs-grid #grid
    [dataSource]="data"
    [editSettings]="editSettings"
    (actionBegin)="onActionBegin($event)">
    <!-- columns -->
  </ejs-grid>`,
  providers: [EditService]
})
export class MultiFieldValidationGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  editSettings = { mode: 'Dialog' };

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }

  validateDateRange(args: any) {
    if (args.data && args.data.EndDate && args.data.StartDate) {
      if (new Date(args.data.EndDate) < new Date(args.data.StartDate)) {
        args.cancel = true;
        this.showError('End date must be after start date');
      }
    }
  }

  onActionBegin(args: any) {
    if (args.requestType === 'save') {
      this.validateDateRange(args);
    }
  }

  showError(message: string) {
    console.error(message);
  }
}
```

### Validation with Rules State

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-validation',
  template: `
    <div>
      <button (click)="updateRules({ minFreight: 50 })">
        Set minimum freight to $50
      </button>

      <ejs-grid [dataSource]="data">
        <e-columns>
          <e-column field="Freight" [editParams]="getFreightEditParams()"></e-column>
        </e-columns>
      </ejs-grid>
    </div>
  `
})
export class GridWithValidationComponent {
  data: any[] = [];
  validationRules: any = {
    minFreight: 10,
    maxFreight: 5000
  };

  validateFreight(value: number): boolean {
    return value >= this.validationRules.minFreight && value <= this.validationRules.maxFreight;
  }

  updateRules(newRules: any) {
    this.validationRules = { ...this.validationRules, ...newRules };
  }

  getFreightEditParams() {
    return {
      params: {
        custom: {
          validator: (value: number) => this.validateFreight(value),
          message: `Freight must be between $${this.validationRules.minFreight} and $${this.validationRules.maxFreight}`
        }
      }
    };
  }
}
```

---

## Async Validation

### Server-Side Unique Check

```typescript
import { HttpClient } from '@angular/common/http';
import { Component } from '@angular/core';

@Component({
  selector: 'app-unique-validator',
  template: `<ejs-grid [dataSource]="data">
    <e-columns>
      <e-column
        field="CustomerID"
        headerText="Customer ID"
        [editParams]="customerIDEditParams">
      </e-column>
    </e-columns>
  </ejs-grid>`
})
export class UniqueValidatorComponent {
  data: any[] = [];
  customerIDEditParams: any;

  constructor(private http: HttpClient) {
    this.customerIDEditParams = {
      params: {
        required: true
      }
    };
  }

  async validateUniqueCustomerID(value: string) {
    try {
      const response = await this.http.get<any>(`/api/customers/exists/${value}`).toPromise();
      return !response.exists;  // true if unique
    } catch (error) {
      return false;
    }
  }
}
```

### Async Validation with Promise

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-async-validation-promise',
  template: `<ejs-grid #grid
    [dataSource]="data"
    [editSettings]="editSettings"
    (actionBegin)="onActionBegin($event)">
    <!-- columns -->
  </ejs-grid>`,
  providers: [EditService]
})
export class AsyncValidationPromiseComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  editSettings = { mode: 'Dialog' };

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }

  validateOrderNumber(value: string): Promise<boolean> {
    return new Promise((resolve) => {
      setTimeout(() => {
        const isValid = value && value.length >= 3;
        resolve(isValid);
      }, 500);
    });
  }

  async onActionBegin(args: any) {
    if (args.requestType === 'save') {
      const isValid = await this.validateOrderNumber(args.data.OrderNumber);
      if (!isValid) {
        args.cancel = true;
        this.showError('Order number must be at least 3 characters');
      }
    }
  }

  showError(message: string) {
    console.error(message);
  }
}
```

### Debounced Async Validation

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-debounced-validator',
  template: '<ejs-grid [dataSource]="data"></ejs-grid>'
})
import { Component, ViewChild, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-debounced-validator',
  template: `<ejs-grid #grid [dataSource]="data">
    <e-columns>
      <e-column
        field="Email"
        headerText="Email"
        [editParams]="emailEditParams">
      </e-column>
    </e-columns>
  </ejs-grid>`,
  providers: [EditService]
})
export class DebouncedValidatorComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  emailEditParams: any;
  private timeoutRef: any;

  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.emailEditParams = {
      params: {
        required: true
      }
    };
  }

  createDebouncedValidator(validationFn: Function, delay: number = 500) {
    return (value: any) => {
      return new Promise((resolve) => {
        clearTimeout(this.timeoutRef);
        this.timeoutRef = setTimeout(async () => {
          const result = await validationFn(value);
          resolve(result);
        }, delay);
      });
    };
  }

  async validateEmail(email: string) {
    try {
      const response = await this.http.get<any>(`/api/email/validate?email=${email}`).toPromise();
      return response.isValid;
    } catch (error) {
      return false;
    }
  }
}
```

---

## Error Display

### Style Validation Errors

```typescript
import { Component } from '@angular/core';
import { EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-validation-error-styling',
  template: `
    <ejs-grid [dataSource]="data">
      <!-- columns -->
    </ejs-grid>
  `,
  styles: [`
    .e-grid .e-gridcontent .e-invalid {
      border: 2px solid #FF6B6B !important;
      background-color: #FFE5E5;
    }

    .e-grid .e-form-group.e-error .e-float-text {
      color: #FF6B6B;
      font-weight: bold;
    }

    .e-grid .e-errorinline {
      color: #FF6B6B;
      font-size: 12px;
      margin-top: 4px;
    }
  `],
  providers: [EditService]
})
export class ValidationErrorStylingComponent {
  data: any[] = [];
}
```

### Custom Error Message Display

```typescript
@Component({
  selector: 'app-error-display',
  template: `
    <ejs-grid [dataSource]="data" [editSettings]="editSettings" (actionFailure)="onActionFailure($event)">
      <!-- columns -->
    </ejs-grid>
  `
})
export class ErrorDisplayGridComponent {
  data: any[] = [];
  editSettings = { mode: 'Dialog' };

  onActionFailure(args: any) {
    if (!args.status) {
      console.error(`Validation Error: ${args.statusMessage}`);
      
      // Show toast notification
      this.showNotification({
        type: 'error',
        message: args.statusMessage,
        duration: 5000
      });
    }
  }

  showNotification(config: any) {
    // Show notification logic
  }
}
```

### Tooltip Error Messages

```typescript
const validationErrorTooltip = `
  .e-grid .e-invalid::after {
    content: attr(title);
    position: absolute;
    background-color: #FF6B6B;
    color: white;
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 12px;
    white-space: nowrap;
    z-index: 1000;
  }
`;

import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-tooltip-validation',
  template: `<ejs-grid #grid [dataSource]="data">
    <e-columns>
      <e-column
        field="Email"
        headerText="Email"
        title="Must be valid email"
        [editParams]="emailEditParams">
      </e-column>
    </e-columns>
  </ejs-grid>`,
  styles: [`${validationErrorTooltip}`]
})
export class TooltipValidationComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  emailEditParams = {
    params: {
      required: true,
      pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    }
  };

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }
}
```

---

## Validation Events

### Validate Before Save

```typescript
@Component({
  selector: 'app-validate-save',
  template: `<ejs-grid #grid
    [dataSource]="data"
    [editSettings]="editSettings"
    (actionBegin)="onActionBegin($event)">
    <!-- columns -->
  </ejs-grid>`,
  providers: [EditService, ToolbarService]
})
export class ValidateSaveGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  editSettings = { mode: 'Dialog', allowEditing: true, allowAdding: true, allowDeleting: true };

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }

  onActionBegin(args: any) {
    if (args.requestType === 'save') {
      if (!args.data.OrderID) {
        args.cancel = true;
        this.showError('Order ID is required');
      }
      
      if (args.data.Freight < 0) {
        args.cancel = true;
        this.showError('Freight cannot be negative');
      }
    
      if (!args.data.CustomerID || args.data.CustomerID.length < 2) {
        args.cancel = true;
        this.showError('Customer ID must be at least 2 characters');
      }
    }
  }

  showError(message: string) {
    console.error(message);
  }
}
```

### Validate After Edit

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-validate-complete',
  template: `<ejs-grid #grid
    [dataSource]="data"
    [editSettings]="editSettings"
    (actionComplete)="onActionComplete($event)">
    <!-- columns -->
  </ejs-grid>`,
  providers: [EditService]
})
export class ValidateCompleteGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  editSettings = { mode: 'Dialog' };

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }

  onActionComplete(args: any) {
    if (args.requestType === 'save') {
      console.log('Record saved successfully');
    }
    
    if (args.requestType === 'cancel') {
      console.log('Edit cancelled');
    }
  }
}
```

### Real-time Field Validation

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-realtime-validation',
  template: `<ejs-grid #grid
    [dataSource]="data"
    [editSettings]="editSettings"
    (actionBegin)="onActionBegin($event)">
    <!-- columns -->
  </ejs-grid>`,
  providers: [EditService]
})
export class RealtimeValidationGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  editSettings = { mode: 'Dialog' };

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }

  onActionBegin(args: any) {
    if (args.requestType === 'beginEdit') {
      this.attachValidationListeners();
    }
  }

  attachValidationListeners() {
    // Get input elements
    const inputs = document.querySelectorAll('.e-gridcontent input');
    
    inputs.forEach((input: any) => {
      input.addEventListener('change', (e: any) => {
        this.validateField(e.target.value, e.target.name);
      });
    });
  }

  validateField(value: any, fieldName: string) {
    let isValid = true;
    let message = '';

    if (fieldName === 'OrderID' && !value) {
      isValid = false;
      message = 'Order ID is required';
    } else if (fieldName === 'Freight' && value < 0) {
      isValid = false;
      message = 'Freight must be positive';
    }

    this.updateValidationStatus(fieldName, isValid, message);
  }

  updateValidationStatus(fieldName: string, isValid: boolean, message: string) {
    // Update validation UI
    console.log(`Field: ${fieldName}, Valid: ${isValid}, Message: ${message}`);
  }
}
```

---

## Server-Side Validation

### Send Data for Server Validation

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-server-validation',
  template: `<ejs-grid #grid
    [dataSource]="data"
    [editSettings]="editSettings"
    (actionBegin)="onActionBegin($event)">
    <!-- columns -->
  </ejs-grid>`,
  providers: [EditService]
})
export class ServerValidationGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];
  editSettings = { mode: 'Dialog', allowEditing: true };

  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    // Load grid data
  }

  async validateAndSaveToServer(data: any) {
    try {
      const response = await this.http.post<any>('/api/orders/validate', data).toPromise();
      
      if (!response.isValid) {
        return {
          isValid: false,
          errors: response.errors
        };
      }

      // If valid, save
      await this.http.post('/api/orders', data).toPromise();
      return { isValid: true };
    } catch (error: any) {
      return {
        isValid: false,
        error: error.message
      };
    }
  }

  async onActionBegin(args: any) {
    if (args.requestType === 'save') {
      const validation = await this.validateAndSaveToServer(args.data);
      
      if (!validation.isValid) {
        args.cancel = true;
        this.showErrors(validation.errors || validation.error);
      }
    }
  }

  showErrors(errors: any) {
    console.error('Validation errors:', errors);
  }
}
```

### ASP.NET Validation Endpoint

```csharp
[HttpPost("validate")]
public IActionResult ValidateOrder([FromBody] OrderRequest request)
{
    var errors = new List<string>();

    if (string.IsNullOrEmpty(request.OrderID))
        errors.Add("Order ID is required");

    if (request.Freight < 0 || request.Freight > 5000)
        errors.Add("Freight must be between 0 and 5000");

    if (string.IsNullOrEmpty(request.CustomerID))
        errors.Add("Customer ID is required");

    // Check for duplicates
    if (OrderExists(request.OrderID))
        errors.Add("Order ID already exists");

    return Ok(new {
        isValid = errors.Count == 0,
        errors = errors
    });
}
```

---

## Validation Checklist

- [ ] Mark required fields clearly
- [ ] Validate data type (number, date, etc.)
- [ ] Set min/max ranges for numeric fields
- [ ] Validate email format
- [ ] Check for unique values (customer ID, email)
- [ ] Validate date logic (start < end)
- [ ] Show clear error messages
- [ ] Style invalid fields distinctly
- [ ] Prevent save if validation fails
- [ ] Test with edge cases
- [ ] Test with special characters
- [ ] Test async validators
- [ ] Provide user feedback on progress
- [ ] Implement server-side validation
- [ ] Log validation errors for debugging






