# Editor and Popup Customization

## Table of Contents
- [Overview](#overview)
- [Default Event Editor](#default-event-editor)
- [Opening the Editor](#opening-the-editor)
- [Customize Editor Header and Button Text](#customize-editor-header-and-button-text)
- [Change Field Labels](#change-field-labels)
- [Field Validation](#field-validation)
- [Add Additional Fields](#add-additional-fields)
- [Customize Event Duration](#customize-event-duration)
- [Prevent Editor Popups](#prevent-editor-popups)
- [Prevent Editor on Cell Double-Click](#prevent-editor-on-cell-double-click)
- [Customize Timezone Collection](#customize-timezone-collection)
- [Custom Editor Template](#custom-editor-template)
- [Complete Editor Template](#complete-editor-template)
- [Save Custom Template Data Without e-field Class](#save-custom-template-data-without-e-field-class)
- [Custom Header and Footer Templates](#custom-header-and-footer-templates)
- [Quick Info Popups](#quick-info-popups)
- [Disable Quick Info](#disable-quick-info)
- [Customize Quick Info Templates](#customize-quick-info-templates)
- [More Events Indicator](#more-events-indicator)
- [Disable More Indicator Popup](#disable-more-indicator-popup)
- [Customize More Indicator Popup](#customize-more-indicator-popup)
- [Navigate to Day View on More Click](#navigate-to-day-view-on-more-click)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Read-Only Scheduler](#read-only-scheduler)
- [Minimal Editor (Subject Only)](#minimal-editor-subject-only)
- [Custom Event Categories](#custom-event-categories)
- [Approval Workflow](#approval-workflow)
- [Required Attendees](#required-attendees)
- [Priority/Urgency Fields](#priorityurgency-fields)

## Overview

The Scheduler provides built-in editors and popups for creating and managing events. Customization options include:
- **Event Editor**: Detailed window for creating/editing appointments
- **Quick Info Popup**: Quick view/edit on cell or event click
- **More Indicator Popup**: Shows additional events when cells overflow
- **Custom Templates**: Complete control over editor appearance and behavior

## Default Event Editor

### Opening the Editor

The editor window opens in different modes:
- **Add Mode**: Double-click an empty cell
- **Edit Mode**: Double-click an existing event
- **Mobile**: Single tap event → Edit icon, or tap cell twice for add mode

**Prevent Editor Opening**:
```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [readonly]='true'
      [eventSettings]='eventSettings'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
}
```

### Customize Editor Header and Button Text

Change header title and button labels using localization:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';
import { L10n } from '@syncfusion/ej2-base';

L10n.load({
  'en-US': {
    'schedule': {
      'saveButton': 'Add',
      'cancelButton': 'Close',
      'deleteButton': 'Remove',
      'newEvent': 'Add Event'
    }
  }
});

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [selectedDate]='selectedDate'
      [eventSettings]='eventSettings'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public selectedDate: Date = new Date(2024, 0, 15);
  
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
}
```

### Change Field Labels

Customize default field labels like "Subject", "Location":

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [selectedDate]='selectedDate'
      [eventSettings]='eventSettings'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public selectedDate: Date = new Date(2024, 0, 15);
  
  public eventSettings: EventSettingsModel = {
    dataSource: [],
    fields: {
      id: 'Id',
      subject: { name: 'Subject', title: 'Event Name' },
      location: { name: 'Location', title: 'Event Location' },
      description: { name: 'Description', title: 'Event Description' },
      startTime: { name: 'StartTime', title: 'Start Duration' },
      endTime: { name: 'EndTime', title: 'End Duration' }
    }
  };
}
```

### Field Validation

Add client-side validation rules to editor fields:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [selectedDate]='selectedDate'
      [eventSettings]='eventSettings'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public selectedDate: Date = new Date(2024, 0, 15);
  
  minValidation = (args: { [key: string]: string }) => {
    return args['value'].length >= 5;
  };
  
  public eventSettings: EventSettingsModel = {
    dataSource: [],
    fields: {
      id: 'Id',
      subject: { 
        name: 'Subject', 
        validation: { required: true } 
      },
      location: { 
        name: 'Location', 
        validation: { required: true } 
      },
      description: {
        name: 'Description',
        validation: {
          required: true,
          minLength: [this.minValidation, 'Need at least 5 letters']
        }
      },
      startTime: { 
        name: 'StartTime', 
        validation: { required: true } 
      },
      endTime: { 
        name: 'EndTime', 
        validation: { required: true } 
      }
    }
  };
}
```

**Available Validation Rules**:
- `required: true`
- `minLength: [validator, message]`
- `maxLength: number`
- `email: true`
- `regex: [pattern, message]`

### Add Additional Fields

Add custom fields to the default editor using `popupOpen` event:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel, PopupOpenEventArgs } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';
import { DropDownList } from '@syncfusion/ej2-dropdowns';
import { createElement } from '@syncfusion/ej2-base';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [selectedDate]='selectedDate'
      [eventSettings]='eventSettings'
      (popupOpen)='onPopupOpen($event)'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public selectedDate: Date = new Date(2024, 0, 15);
  
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
  
  onPopupOpen(args: PopupOpenEventArgs): void {
    if (args.type === 'Editor') {
      // Create custom field only once
      if (!args.element.querySelector('.custom-field-row')) {
        let row: HTMLElement = createElement('div', { className: 'custom-field-row' });
        let formElement: HTMLElement = args.element.querySelector('.e-schedule-form') as HTMLElement;
        formElement.firstChild?.insertBefore(row, args.element.querySelector('.e-title-location-row'));
        
        let container: HTMLElement = createElement('div', { className: 'custom-field-container' });
        let inputEle: HTMLInputElement = createElement('input', {
          className: 'e-field',
          attrs: { name: 'EventType' }
        }) as HTMLInputElement;
        container.appendChild(inputEle);
        row.appendChild(container);
        
        let dropDownList: DropDownList = new DropDownList({
          dataSource: [
            { text: 'Public Event', value: 'public-event' },
            { text: 'Maintenance', value: 'maintenance' },
            { text: 'Commercial Event', value: 'commercial-event' },
            { text: 'Family Event', value: 'family-event' }
          ],
          fields: { text: 'text', value: 'value' },
          value: (args.data as any)['EventType'] as string,
          floatLabelType: 'Always',
          placeholder: 'Event Type'
        });
        dropDownList.appendTo(inputEle);
      }
    }
  }
}
```

**Key Points**:
- Add `e-field` class for automatic data processing
- Use `popupOpen` event to inject custom elements
- Check if element exists to avoid duplicates

### Customize Event Duration

Change default event duration (default: 60 minutes from timeScale.interval):

```typescript
onPopupOpen(args: PopupOpenEventArgs): void {
  if (args.type === 'Editor') {
    args.duration = 30; // 30 minutes
  }
}
```

### Prevent Editor Popups

Prevent specific popup types from opening:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel, PopupOpenEventArgs } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [eventSettings]='eventSettings'
      (popupOpen)='onPopupOpen($event)'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
  
  onPopupOpen(args: PopupOpenEventArgs): void {
    // Prevent specific popups
    if (args.type === 'Editor' || args.type === 'QuickInfo') {
      args.cancel = true;
    }
  }
}
```

**Popup Types**:
- `'Editor'`: Detailed editor window
- `'QuickInfo'`: Quick popup on cell click
- `'EditEventInfo'`: Quick popup on event click
- `'ViewEventInfo'`: Quick popup in responsive mode
- `'EventContainer'`: More event indicator popup
- `'RecurrenceAlert'`: Edit recurrence alert
- `'DeleteAlert'`: Delete confirmation
- `'ValidationAlert'`: Validation alert
- `'RecurrenceValidationAlert'`: Recurrence validation alert

### Prevent Editor on Cell Double-Click

Block editor opening specifically on cell double-click:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel, CellClickEventArgs } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [eventSettings]='eventSettings'
      (cellDoubleClick)='onCellDoubleClick($event)'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
  
  onCellDoubleClick(args: CellClickEventArgs): void {
    args.cancel = true;
    // Implement custom behavior here
  }
}
```

### Customize Timezone Collection

Show limited timezone options in the editor:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [selectedDate]='selectedDate'
      [timezoneDataSource]='timezoneDataSource'
      [eventSettings]='eventSettings'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public selectedDate: Date = new Date(2024, 0, 15);
  
  public timezoneDataSource: { Value: string, Text: string }[] = [
    { Value: 'Pacific/Honolulu', Text: 'Hawaii Time' },
    { Value: 'America/Los_Angeles', Text: 'Pacific Time' },
    { Value: 'America/Chicago', Text: 'Central Time' },
    { Value: 'America/New_York', Text: 'Eastern Time' },
    { Value: 'UTC', Text: 'UTC' },
    { Value: 'Europe/London', Text: 'London' },
    { Value: 'Asia/Kolkata', Text: 'India' }
  ];
  
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
}
```

## Custom Editor Template

### Complete Editor Template

Replace the default editor with a fully customized template:

```typescript
import { Component, ViewChild } from '@angular/core';
import { ScheduleComponent, ScheduleModule, EventSettingsModel, PopupOpenEventArgs } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';
import { DateTimePickerModule } from '@syncfusion/ej2-angular-calendars';
import { DropDownListModule } from '@syncfusion/ej2-angular-dropdowns';
import { isNullOrUndefined } from '@syncfusion/ej2-base';
import { ChangeEventArgs } from '@syncfusion/ej2-calendars';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule, DateTimePickerModule, DropDownListModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      #schedule
      width='100%' 
      height='550px'
      [selectedDate]='selectedDate'
      [eventSettings]='eventSettings'
      [showQuickInfo]='false'>
      <ng-template #editorTemplate let-data>
        <table class="custom-event-editor" width="100%" cellpadding="5">
          <tbody>
            <tr>
              <td class="e-textlabel">Summary</td>
              <td colspan="4">
                <input 
                  id="Subject" 
                  class="e-field e-input" 
                  type="text" 
                  name="Subject" 
                  style="width: 100%" />
              </td>
            </tr>
            <tr>
              <td class="e-textlabel">Status</td>
              <td colspan="4">
                <ejs-dropdownlist 
                  id='EventType' 
                  class="e-field" 
                  data-name="EventType" 
                  placeholder='Choose Status'
                  [dataSource]='statusData' 
                  value="{{data.EventType}}">
                </ejs-dropdownlist>
              </td>
            </tr>
            <tr>
              <td class="e-textlabel">From</td>
              <td colspan="4">
                <ejs-datetimepicker 
                  id="StartTime" 
                  class="e-field" 
                  data-name="StartTime" 
                  format="M/dd/yy h:mm a"
                  (change)="onDateChange($event)" 
                  [value]="startDateParser(data.startTime || data.StartTime)">
                </ejs-datetimepicker>
              </td>
            </tr>
            <tr>
              <td class="e-textlabel">To</td>
              <td colspan="4">
                <ejs-datetimepicker 
                  id="EndTime" 
                  class="e-field" 
                  data-name="EndTime" 
                  format="M/dd/yy h:mm a"
                  (change)="onDateChange($event)" 
                  [value]="endDateParser(data.endTime || data.EndTime)">
                </ejs-datetimepicker>
              </td>
            </tr>
            <tr>
              <td class="e-textlabel">Reason</td>
              <td colspan="4">
                <textarea 
                  id="Description" 
                  class="e-field e-input" 
                  name="Description" 
                  rows="3" 
                  cols="50"
                  value="{{data.Description}}" 
                  style="width: 100%; height: 60px !important; resize: vertical">
                </textarea>
              </td>
            </tr>
          </tbody>
        </table>
      </ng-template>
    </ejs-schedule>
  `
})
export class AppComponent {
  @ViewChild('schedule') public scheduleObj!: ScheduleComponent;
  
  public selectedDate: Date = new Date(2024, 0, 15);
  public startDate!: Date;
  public endDate!: Date;
  
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
  
  public statusData: Object[] = ['New', 'Requested', 'Confirmed'];
  
  startDateParser(data: string) {
    if (isNullOrUndefined(this.startDate) && !isNullOrUndefined(data)) {
      return new Date(data);
    } else if (!isNullOrUndefined(this.startDate)) {
      return new Date(this.startDate);
    }
    return new Date();
  }
  
  endDateParser(data: string) {
    if (isNullOrUndefined(this.endDate) && !isNullOrUndefined(data)) {
      return new Date(data);
    } else if (!isNullOrUndefined(this.endDate)) {
      return new Date(this.endDate);
    }
    return new Date();
  }
  
  onDateChange(args: ChangeEventArgs): void {
    if (!isNullOrUndefined(args.event)) {
      if (args.element.id === 'StartTime') {
        this.startDate = args.value as Date;
      } else if (args.element.id === 'EndTime') {
        this.endDate = args.value as Date;
      }
    }
  }
}
```

**Template Requirements**:
- Add `e-field` class to all form elements for automatic processing
- Use `data-name` attribute to map fields
- Supported components: DropDownList, DateTimePicker, MultiSelect, DatePicker, CheckBox, TextBox

### Save Custom Template Data Without e-field Class

Handle data manually using `popupClose` event:

```typescript
import { Component, ViewChild, ElementRef } from '@angular/core';
import { ScheduleModule, PopupCloseEventArgs } from '@syncfusion/ej2-angular-schedule';
import { isNullOrUndefined } from '@syncfusion/ej2-base';

@Component({
  selector: 'app-root',
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      [eventSettings]='eventSettings'
      (popupClose)='onPopupClose($event)'>
      <ng-template #editorTemplate let-data>
        <input 
          id="Subject" 
          type="text" 
          #subject 
          value="{{data.Subject}}" />
      </ng-template>
    </ejs-schedule>
  `
})
export class AppComponent {
  @ViewChild('subject') public subject!: ElementRef<HTMLInputElement>;
  
  public eventSettings = { dataSource: [] };
  
  onPopupClose(args: PopupCloseEventArgs): void {
    if (args.type === 'Editor' && !isNullOrUndefined(args.data)) {
      // Save only on 'save' and 'delete', not 'close' or 'cancel'
      if (this.subject.nativeElement) {
        (args.data as any)['Subject'] = this.subject.nativeElement.value;
      }
    }
  }
}
```

### Custom Header and Footer Templates

Customize editor window header and footer:

```typescript
import { Component, ViewChild } from '@angular/core';
import { ScheduleComponent, ScheduleModule, PopupOpenEventArgs } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule, CommonModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      #schedule
      width='100%' 
      height='550px'
      [eventSettings]='eventSettings'
      (popupOpen)='onPopupOpen($event)'>
      <ng-template #editorHeaderTemplate let-data>
        <div *ngIf="data.Subject; else createNewEvent">
          {{ data.Subject }}
        </div>
        <ng-template #createNewEvent>
          Create New Event
        </ng-template>
      </ng-template>
      <ng-template #editorFooterTemplate>
        <div id="verify">
          <input type="checkbox" id="check-box" value="unchecked">
          <label id="text">Verified</label>
        </div>
        <div id="right-button">
          <button id="Save" class="e-control e-btn e-primary" disabled>Save</button>
          <button id="Cancel" class="e-control e-btn e-primary">Cancel</button>
        </div>
      </ng-template>
    </ejs-schedule>
  `
})
export class AppComponent {
  @ViewChild('schedule') public scheduleObj!: ScheduleComponent;
  
  public eventSettings = { dataSource: [] };
  
  onPopupOpen(args: PopupOpenEventArgs): void {
    if (args.type === 'Editor') {
      const saveButton = args.element.querySelector('#Save') as HTMLElement;
      const cancelButton = args.element.querySelector('#Cancel') as HTMLElement;
      const checkBox = args.element.querySelector('#check-box') as HTMLInputElement;
      
      checkBox.onchange = () => {
        if (checkBox.checked) {
          saveButton.removeAttribute('disabled');
        } else {
          saveButton.setAttribute('disabled', '');
        }
      };
      
      saveButton.onclick = () => {
        // Save logic
        this.scheduleObj.closeEditor();
      };
      
      cancelButton.onclick = () => {
        this.scheduleObj.closeEditor();
      };
    }
  }
}
```

## Quick Info Popups

### Disable Quick Info

Hide all quick popups:

```typescript
<ejs-schedule 
  width='100%' 
  height='550px'
  [showQuickInfo]='false'
  [eventSettings]='eventSettings'>
</ejs-schedule>
```

### Customize Quick Info Templates

Use `quickInfoTemplates` to customize quick popup content:

```typescript
import { Component, ViewChild } from '@angular/core';
import { ScheduleComponent, ScheduleModule, EventSettingsModel, CurrentAction } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';
import { isNullOrUndefined } from '@syncfusion/ej2-base';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      #schedule
      width='100%' 
      height='550px'
      [eventSettings]='eventSettings'>
      <ng-template #quickInfoTemplatesHeader let-data>
        <div class="quick-info-header">
          <span class="e-subject">{{data.Subject}}</span>
        </div>
      </ng-template>
      <ng-template #quickInfoTemplatesContent let-data>
        <div class="quick-info-content">
          <div><strong>Time:</strong> {{formatTime(data.StartTime)}} - {{formatTime(data.EndTime)}}</div>
          <div *ngIf="data.Location"><strong>Location:</strong> {{data.Location}}</div>
          <div *ngIf="data.Description"><strong>Description:</strong> {{data.Description}}</div>
        </div>
      </ng-template>
      <ng-template #quickInfoTemplatesFooter let-data>
        <div class="quick-info-footer">
          <button class="e-btn e-flat" (click)="onEditClick()">Edit</button>
          <button class="e-btn e-flat" (click)="onDeleteClick()">Delete</button>
          <button class="e-btn e-flat" (click)="onCloseClick()">Close</button>
        </div>
      </ng-template>
    </ejs-schedule>
  `
})
export class AppComponent {
  @ViewChild('schedule') public scheduleObj!: ScheduleComponent;
  
  private selectionTarget: Element | null = null;
  
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
  
  formatTime(date: Date): string {
    return new Date(date).toLocaleTimeString('en-US', { 
      hour: '2-digit', 
      minute: '2-digit' 
    });
  }
  
  onEditClick(): void {
    this.onCloseClick();
    if (this.selectionTarget) {
      const eventData = this.scheduleObj.getEventDetails(this.selectionTarget);
      this.scheduleObj.openEditor(eventData, 'Save');
    }
  }
  
  onDeleteClick(): void {
    this.onCloseClick();
    if (this.selectionTarget) {
      const eventData = this.scheduleObj.getEventDetails(this.selectionTarget);
      this.scheduleObj.deleteEvent(eventData, 'Delete');
    }
  }
  
  onCloseClick(): void {
    this.scheduleObj.quickPopup.quickPopupHide();
  }
}
```

**Template Properties**:
- `quickInfoTemplatesHeader`: Custom header content
- `quickInfoTemplatesContent`: Custom body content
- `quickInfoTemplatesFooter`: Custom footer buttons

## More Events Indicator

### Disable More Indicator Popup

Prevent the "+N more" popup from opening:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel, PopupOpenEventArgs } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      currentView='Month'
      [eventSettings]='eventSettings'
      (popupOpen)='onPopupOpen($event)'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data with many events
  };
  
  onPopupOpen(args: PopupOpenEventArgs): void {
    if (args.type === 'EventContainer') {
      args.cancel = true;
    }
  }
}
```

### Customize More Indicator Popup

Modify the popup header to show event count:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel, PopupOpenEventArgs } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';
import { Internationalization } from '@syncfusion/ej2-base';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      width='100%' 
      height='550px'
      currentView='Month'
      [eventSettings]='eventSettings'
      (popupOpen)='onPopupOpen($event)'>
    </ejs-schedule>
  `
})
export class AppComponent {
  private instance: Internationalization = new Internationalization();
  
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
  
  onPopupOpen(args: PopupOpenEventArgs): void {
    if (args.type === 'EventContainer') {
      const dateStr = this.instance.formatDate((args.data as any).date, { skeleton: 'MMMEd' });
      const eventCount = (args.data as any).event.length;
      
      (args.element.querySelector('.e-header-date') as HTMLElement).innerText = dateStr;
      (args.element.querySelector('.e-header-day') as HTMLElement).innerText = `Event count: ${eventCount}`;
    }
  }
}
```

### Navigate to Day View on More Click

Replace popup with navigation:

```typescript
import { Component } from '@angular/core';
import { ScheduleModule, EventSettingsModel, MoreEventsClickArgs } from '@syncfusion/ej2-angular-schedule';
import { DayService, WeekService, MonthService } from '@syncfusion/ej2-angular-schedule';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ScheduleModule],
  providers: [DayService, WeekService, MonthService],
  template: `
    <ejs-schedule 
      #schedule
      width='100%' 
      height='550px'
      currentView='Month'
      [selectedDate]='selectedDate'
      [eventSettings]='eventSettings'
      (moreEventsClick)='onMoreEventsClick($event)'>
    </ejs-schedule>
  `
})
export class AppComponent {
  public selectedDate: Date = new Date(2024, 0, 15);
  
  public eventSettings: EventSettingsModel = {
    dataSource: [] // Your event data
  };
  
  onMoreEventsClick(args: MoreEventsClickArgs): void {
    args.cancel = true; // Prevent popup
    args.isPopupOpen = false;
    
    // Navigate to Day view for the clicked date
    (args.event.currentTarget as HTMLElement).classList.add('e-navigate');
  }
}
```

## Best Practices

1. **e-field Class**: Always add for automatic data processing in templates
2. **Validation**: Apply validation rules to prevent invalid data
3. **PopupOpen Event**: Use for dynamic customization based on context
4. **PopupClose Event**: Handle manual data extraction when not using e-field
5. **Localization**: Use L10n for multi-language support
6. **Timezone**: Provide limited, relevant timezone options
7. **Quick Info**: Disable if using custom editors for consistency
8. **More Indicator**: Customize or disable based on UX requirements
9. **Mobile**: Test quick info behavior on touch devices
10. **Performance**: Avoid heavy processing in popupOpen event

## Common Scenarios

### Read-Only Scheduler
```typescript
[readonly]='true'
```

### Minimal Editor (Subject Only)
```typescript
fields: {
  subject: { name: 'Subject', validation: { required: true } }
}
```

### Custom Event Categories
Add dropdown with predefined categories using additional fields.

### Approval Workflow
Use custom footer template with "Approve" and "Reject" buttons.

### Required Attendees
Add MultiSelect for attendees with validation.

### Priority/Urgency Fields
Add DropDownList or Rating component for priority levels.
