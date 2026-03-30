---
name: syncfusion-angular-textbox
description: Master Syncfusion Angular TextBox component implementation with floating labels, validation states, adornments, and accessibility support. Guide covers setup, input features, styling, multiline handling, and form integration patterns.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Inputs"
---

# Implementing Syncfusion Angular TextBox

The Syncfusion Angular TextBox component is a feature-rich input element that enhances the native HTML input with floating labels, validation states, adornments (prepended/appended elements), accessibility support, and comprehensive styling options. This skill guides you through implementation patterns, configuration, and best practices.

## Component Overview

The TextBox component provides:

| Feature | Purpose |
|---------|---------|
| **Floating Labels** | Animated labels that float above input when focused or filled |
| **Validation States** | Visual feedback (error, warning, success) with CSS classes |
| **Adornments** | Prepend/append custom HTML elements (icons, buttons, units) |
| **Clear Button** | Built-in clear functionality to reset input value |
| **Disabled/Read-only States** | Control user interaction and editability |
| **HTML Attributes** | Support for standard input attributes (type, maxlength, etc.) |
| **Multiline Support** | Textarea configuration with row/column sizing |
| **Accessibility** | WCAG 2.2 compliance, keyboard navigation, ARIA attributes |
| **RTL Support** | Right-to-left language support |
| **Styling Options** | CSS classes, validation colors, responsive sizing |

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- Create your first TextBox component
- CSS imports and theme selection
- Floating label implementation
- Basic event binding and data binding
- Common setup issues and solutions

### Input Features and State Management
📄 **Read:** [references/input-features.md](references/input-features.md)
- Clear button implementation (showClearButton)
- Disabled state (enabled property)
- Read-only state (readonly property)
- HTML attributes configuration (htmlAttributes)
- Supporting input types and attributes
- State management patterns for forms

### Adornments and Customization
📄 **Read:** [references/adornments-customization.md](references/adornments-customization.md)
- Prepend and append template usage
- Icon adornments for visual context
- Button adornments (password toggle, clear)
- Validation status indicators
- Unit indicators (currency, temperature, etc.)
- Performance and accessibility considerations

### Validation States and Error Handling
📄 **Read:** [references/validation-states.md](references/validation-states.md)
- Error, warning, and success validation states
- CSS class approach (e-error, e-warning, e-success)
- Visual feedback patterns
- Adding asterisk for required fields
- Form integration with validation
- Custom error message display

### Styling and Appearance Customization
📄 **Read:** [references/styling-appearance.md](references/styling-appearance.md)
- CSS structure and class hierarchy
- Basic sizing (height, font, padding)
- Floating label color customization
- Validation state color changes
- Borders, rounded corners, and advanced styling
- Dynamic styling based on input value
- Theme integration and customization

### Multiline and Sizing Features
📄 **Read:** [references/multiline-sizing.md](references/multiline-sizing.md)
- Multiline textarea configuration
- Row and column sizing
- Responsive sizing patterns
- Height adjustments and constraints
- Character counting implementation
- Text wrapping and overflow handling

### Accessibility and Migration
📄 **Read:** [references/accessibility-migration.md](references/accessibility-migration.md)
- WCAG 2.2 and Section 508 compliance
- Keyboard navigation support
- ARIA attributes (aria-labelledby, aria-invalid, aria-disabled)
- Screen reader compatibility
- Migration from CSS TextBox to Angular component
- RTL and mobile accessibility

---

## Quick Start Example

**Create a floating label TextBox:**

```typescript
import { Component } from '@angular/core';
import { TextBoxModule } from '@syncfusion/ej2-angular-inputs';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [TextBoxModule],
  template: `
    <div style="margin: 50px;">
      <h2>Angular TextBox Example</h2>
      <ejs-textbox 
        #textbox
        [floatLabelType]="'Auto'"
        placeholder="Enter your name"
        (input)="onInput($event)"
      ></ejs-textbox>
      <p>Value: {{ textValue }}</p>
    </div>
  `
})
export class AppComponent {
  textValue = '';

  onInput(event: any) {
    this.textValue = event.target.value;
  }
}
```

**Key Points:**
- Use `floatLabelType="'Auto'"` for automatic floating labels
- Import `TextBoxModule` from `@syncfusion/ej2-angular-inputs`
- Use standard Angular `(input)` event binding
- Set `placeholder` for the floating label text

---

## Common Patterns

### Pattern 1: Email Input with Icon Adornment
```typescript
<ejs-textbox
  placeholder="Email"
  [floatLabelType]="'Auto'"
  [appendTemplate]="'appendTemplate'"
></ejs-textbox>
<ng-template #appendTemplate>
  <span class="e-input-group-icon">✉</span>
</ng-template>
```
**When to Use:** Email, username, or other fields with visual context

### Pattern 2: Password Toggle
```typescript
<ejs-textbox
  [type]="passwordVisible ? 'text' : 'password'"
  placeholder="Password"
  [floatLabelType]="'Auto'"
  [appendTemplate]="'toggleTemplate'"
></ejs-textbox>
<ng-template #toggleTemplate>
  <button (click)="togglePassword()">👁</button>
</ng-template>

// Component
togglePassword() {
  this.passwordVisible = !this.passwordVisible;
}
```
**When to Use:** Password fields requiring visibility toggle

### Pattern 3: Validation with Error Display
```typescript
<ejs-textbox
  [cssClass]="isValid ? 'e-success' : 'e-error'"
  placeholder="Phone"
  (change)="validatePhone($event)"
></ejs-textbox>
<p *ngIf="!isValid" style="color: red;">{{ errorMessage }}</p>
```
**When to Use:** Form fields with validation feedback

### Pattern 4: Currency Input with Unit Indicator
```typescript
<ejs-textbox
  type="number"
  placeholder="Amount"
  [prependTemplate]="'prependTemplate'"
></ejs-textbox>
<ng-template #prependTemplate>
  <span style="padding: 0 8px;">$</span>
</ng-template>
```
**When to Use:** Currency, temperature, or measurement inputs

---

## Key Props and Configuration

Refer to the full API summary in [references/api.md](references/api.md).

| Property / Method | Type | Purpose |
|------------------|------|--------|
| `floatLabelType` | string | 'Auto' | 'Always' | 'Never' - Controls floating label behavior |
| `placeholder` | string | Text shown when empty; used for floating labels |
| `value` | string | Current input value |
| `enabled` | boolean | Enable/disable the input (default: true) |
| `readonly` | boolean | Make input read-only (selectable but not editable) |
| `showClearButton` | boolean | Display clear button when field has content |
| `cssClass` | string | Custom CSS classes (e.g., 'e-error', 'e-warning', 'e-success', 'e-small', 'e-bigger', 'e-outline', 'e-corner') |
| `htmlAttributes` | object | Standard HTML attributes (name, maxlength, type, etc.) |
| `prependTemplate` | template | Template for content prepended before the input |
| `appendTemplate` | template | Template for content appended after the input |
| `multiline` | boolean | Enable textarea mode (renders a `<textarea>`) |
| `addIcon(position, icons)` | method | Add icon(s) programmatically (`position` = 'append'|'prepend') |
| `addAttributes(attributes)` | method | Add HTML attributes programmatically (e.g., `maxlength`) |
| `removeAttributes(names[])` | method | Remove previously added attributes |
| `focusIn()` / `focusOut()` | method | Programmatically focus or blur the component |
| `destroy()` | method | Destroy the component instance and detach handlers |
| `getPersistData()` | method | Return persisted state string (when `enablePersistence` is used) |

**Notes:**
- Use CSS class `e-corner` together with `e-outline` to show rounded corners for box-model TextBoxes.
- `rows` and `cols` are **not** component properties. To set them on a multiline TextBox, use `addAttributes({rows: '5'} as any)` in the `(created)` event handler (see `references/multiline-sizing.md`).
- For programmatic input creation (dynamic forms), use `Input.createInput` from `@syncfusion/ej2-inputs` (see `references/input-features.md`).

---

## Common Use Cases

### 1. **Contact Form**
Multiple TextBox fields with floating labels, validation states, and required field indicators. See [validation-states.md](references/validation-states.md) and [accessibility-migration.md](references/accessibility-migration.md).

### 2. **Search Input with Clear Button**
TextBox with `showClearButton=true` for quick input reset. See [input-features.md](references/input-features.md).

### 3. **Styled Input with Icon Prefix/Suffix**
TextBox with `prependTemplate` or `appendTemplate` for visual context. See [adornments-customization.md](references/adornments-customization.md).

### 4. **Password Field with Toggle**
Password input with visibility toggle button via append template. See [adornments-customization.md](references/adornments-customization.md).

### 5. **Multiline Comment Field**
Textarea with row sizing and character counting. See [multiline-sizing.md](references/multiline-sizing.md).

### 6. **Accessible Form Field**
TextBox with proper ARIA attributes and keyboard support for compliance. See [accessibility-migration.md](references/accessibility-migration.md).

---

## Related Documentation

- **Syncfusion Angular Inputs**: https://ej2.syncfusion.com/angular/documentation/textbox
- **TextBox API Reference**: https://ej2.syncfusion.com/angular/documentation/api/textbox/
- **Angular Input Guide**: [Angular Official Docs](https://angular.io/guide/forms)
- **WCAG Accessibility**: https://www.w3.org/TR/WCAG22/

---

## Next Steps

1. Start with [getting-started.md](references/getting-started.md) to set up your first TextBox
2. Explore [input-features.md](references/input-features.md) for state management
3. Use [adornments-customization.md](references/adornments-customization.md) for custom UI
4. Reference [validation-states.md](references/validation-states.md) for form validation
5. Customize styling with [styling-appearance.md](references/styling-appearance.md)
6. Handle advanced cases in [multiline-sizing.md](references/multiline-sizing.md) and [accessibility-migration.md](references/accessibility-migration.md)

