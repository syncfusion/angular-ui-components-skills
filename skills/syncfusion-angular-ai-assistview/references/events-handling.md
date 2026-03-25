# Event Handling and User Interactions

## Table of Contents
- [Component Lifecycle Events](#component-lifecycle-events)
- [User Input Events](#user-input-events)
- [File Upload Events](#file-upload-events)
- [Event Arguments Reference](#event-arguments-reference)
- [Event Handling Patterns](#event-handling-patterns)

---

## Component Lifecycle Events

### Created Event

The `created` event is triggered when the AI AssistView component rendering is completed. This is useful for initialization logic and accessing component instance.

```ts
import { AIAssistViewModule } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    // specifies the template string for the AI AssistView component
    template: `<div ejs-aiassistview #aiAssistViewComponent (created)="onCreated()"></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onCreated = () => {
        // Your required action here
    };
}
```

---

## User Input Events

### PromptRequest Event

The `promptRequest` event is triggered when a user sends a prompt request in the AI AssistView component. This is where you handle user queries and generate responses.

**Event Arguments:** `PromptRequestEventArgs`

```ts
import { AIAssistViewModule, PromptRequestEventArgs } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    template: `<div ejs-aiassistview #aiAssistViewComponent (promptRequest)="onPromptRequest($event)"></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onPromptRequest = (args: PromptRequestEventArgs) => {
        console.log('Prompt:', args.prompt);
        console.log('Attached files:', args.attachedFiles);
        console.log('Prompt suggestions:', args.promptSuggestions);
        
        // Cancel the request if needed
        if (args.prompt.length < 3) {
            args.cancel = true;
            alert('Please enter at least 3 characters');
            return;
        }
        
        // Process the prompt
**Event Arguments:** `PromptChangedEventArgs`

```ts
import { AIAssistViewModule, PromptChangedEventArgs } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    template: `<div ejs-aiassistview #aiAssistViewComponent (promptChanged)="onPromptChanged($event)"></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onPromptChanged = (args: PromptChangedEventArgs) => {
        console.log('Current value:', args.value);
        console.log('Previous value:', args.previousValue);
        
        // Real-time validation
        if (args.value.length > 1000) {
            alert('Prompt is too long. Maximum 1000 characters allowed.');
        }
        
        // Auto-suggestions based on input
        if (args.value.startsWith('/')) {
            this.showCommandSuggestions(args.value);
        }
    };
    
    private showCommandSuggestions(value: string) {
        // Show command suggestions
    }
}
```

**Available Properties in PromptChangedEventArgs:**
- `value` (string) - Current value of the prompt
- `previousValue` (string) - Previous value before the change
- `element` (HTMLElement) - HTML element of the text area container
- `event` (Event) - Underlying DOM event
- `name` (string) - Event nameort { AIAssistViewModule } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    // specifies the template string for the AI AssistView component
    template: `<div ejs-aiassistview #aiAssistViewComponent (promptChanged)="onPromptChanged()"></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onPromptChanged = () => {
        // Your required action here
    };
}
```

---

## File Upload Events

### BeforeAttachmentUpload Event

The `beforeAttachmentUpload` event is triggered before attached files begin uploading. Use this for validation of file type, size, or canceling the upload.

```ts
import { AIAssistViewModule } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';
import { UploadingEventArgs } from '@syncfusion/ej2-inputs';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    // specifies the template string for the AI AssistView component
    template: `<div ejs-aiassistview #aiAssistViewComponent [enableAttachments]="enableAttachments" [attachmentSettings]="attachmentSettings" (promptRequest)="onPromptRequest()" (beforeAttachmentUpload)="onBeforeAttachmentUpload($event)" ></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onBeforeAttachmentUpload = (args: UploadingEventArgs) => {
        // Your required action here
    };
    public onPromptRequest = () => {
        setTimeout(() => {
          let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
          this.aiAssistViewComponent.addPromptResponse(defaultResponse);
        }, 1000);
    };
    public enableAttachments: boolean = true;
    public attachmentSettings = {
        saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
        removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove'
    };
}
```

### AttachmentUploadSuccess Event

The `attachmentUploadSuccess` event is triggered when an attached file is successfully uploaded. Use this for post-upload processing or confirmation.

```ts
import { AIAssistViewModule } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';
import { UploadingEventArgs } from '@syncfusion/ej2-inputs';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    // specifies the template string for the AI AssistView component
    template: `<div ejs-aiassistview #aiAssistViewComponent [enableAttachments]="enableAttachments" [attachmentSettings]="attachmentSettings" (promptRequest)="onPromptRequest()" (attachmentUploadSuccess)="onAttachmentUploadSuccess($event)" ></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onAttachmentUploadSuccess = (args: UploadingEventArgs) => {
        // Your required action here
    };
    public onPromptRequest = () => {
        setTimeout(() => {
          let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
          this.aiAssistViewComponent.addPromptResponse(defaultResponse);
        }, 1000);
    };
    public enableAttachments: boolean = true;
    public attachmentSettings = {
        saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
        removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove'
    };
}
```

---

## Additional Events

### AttachmentUploadFailure Event

The `attachmentUploadFailure` event is triggered when an attached file upload fails in the AI AssistView.

```ts
import { AIAssistViewModule } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';
import { UploadingEventArgs } from '@syncfusion/ej2-inputs';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    // specifies the template string for the AI AssistView component
    template: `<div ejs-aiassistview #aiAssistViewComponent [enableAttachments]="enableAttachments" [attachmentSettings]="attachmentSettings" (promptRequest)="onPromptRequest()" (created)="onCreated()" (attachmentUploadFailure)="onAttachmentUploadFailure($event)" ></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onAttachmentUploadFailure = (args: UploadingEventArgs) => {
        // Your required action here
    };
    public onPromptRequest = () => {
        setTimeout(() => {
          let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
          this.aiAssistViewComponent.addPromptResponse(defaultResponse);
        }, 1000);
    };
    public enableAttachments: boolean = true;
    public attachmentSettings = {
        saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
        removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove'
    };
    public onCreated = () => {
        // Your required action here
    };
}
```

### AttachmentRemoved Event

The `attachmentRemoved` event is triggered when an attached file is removed from the AI AssistView.

```ts
import { AIAssistViewModule } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';
import { UploadingEventArgs } from '@syncfusion/ej2-inputs';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    // specifies the template string for the AI AssistView component
    template: `<div ejs-aiassistview #aiAssistViewComponent [enableAttachments]="enableAttachments" [attachmentSettings]="attachmentSettings" (promptRequest)="onPromptRequest()" (attachmentRemoved)="onAttachmentRemoved($event)" ></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onAttachmentRemoved = (args: UploadingEventArgs) => {
        // Your required action here
    };
    public onPromptRequest = () => {
        setTimeout(() => {
          let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
          this.aiAssistViewComponent.addPromptResponse(defaultResponse);
        }, 1000);
    };
    public enableAttachments: boolean = true;
    public attachmentSettings = {
        saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
        removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove'
    };
}
```
---

## Event Arguments Reference

This section provides detailed information about all event argument interfaces used in the AI AssistView component.

### PromptRequestEventArgs

Provides information when a prompt request is made.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `prompt` | string | The text of the prompt request sent by the user |
| `attachedFiles` | FileInfo[] | Array of files attached with the prompt request |
| `promptSuggestions` | string[] | List of prompt suggestions to assist the user |
| `responseToolbarItems` | ToolbarItemModel[] | Toolbar items displayed alongside the response view |
| `cancel` | boolean | Set to true to cancel the prompt request |
| `name` | string | Name of the event |

**Example Usage:**

```ts
public onPromptRequest = (args: PromptRequestEventArgs) => {
    // Access prompt text
    console.log('User prompt:', args.prompt);
    
    // Check for attached files
    if (args.attachedFiles && args.attachedFiles.length > 0) {
        console.log('Files attached:', args.attachedFiles.length);
        args.attachedFiles.forEach(file => {
            console.log('File:', file.name, file.size);
        });
    }
    
    // Cancel if prompt is too short
    if (args.prompt.trim().length < 5) {
        args.cancel = true;
        alert('Please enter at least 5 characters');
        return;
    }
    
    // Add custom response toolbar items
    args.responseToolbarItems = [
        { type: 'Button', iconCss: 'e-icons e-copy', tooltip: 'Copy response' },
        { type: 'Button', iconCss: 'e-icons e-refresh', tooltip: 'Regenerate' }
    ];
    
    // Process the prompt
    this.callAIService(args.prompt, args.attachedFiles);
};
```

---

### PromptChangedEventArgs

Provides information when the prompt text changes.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `value` | string | Current value of the prompt after the change |
| `previousValue` | string | Previous value of the prompt before the change |
| `element` | HTMLElement | HTML element of the text area container |
| `event` | Event | Underlying DOM event that triggered the change |
| `name` | string | Name of the event |

**Example Usage:**

```ts
public onPromptChanged = (args: PromptChangedEventArgs) => {
    // Track changes
    console.log('Changed from:', args.previousValue);
    console.log('Changed to:', args.value);
    
    // Character count validation
    const maxLength = 1000;
    if (args.value.length > maxLength) {
        alert(`Prompt exceeds maximum length of ${maxLength} characters`);
    }
    
    // Show character counter
    this.characterCount = args.value.length;
    
    // Auto-save draft
    if (args.value.length > 10) {
        this.saveDraft(args.value);
    }
    
    // Command detection
    if (args.value.startsWith('/')) {
        this.showCommandPalette(args.value);
    }
    
    // Direct element manipulation if needed
    if (args.element) {
        console.log('Text area element:', args.element);
    }
};
```

---

### StopRespondingEventArgs

Provides information when the 'Stop Responding' button is clicked during an ongoing response.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `prompt` | string | The prompt text associated with the request |
| `dataIndex` | number | Index of the prompt in the prompt list |
| `event` | Event | Underlying DOM event that triggered the action |
| `name` | string | Name of the event |

**Example Usage:**

```ts
import { AIAssistViewModule, StopRespondingEventArgs } from '@syncfusion/ej2-angular-interactive-chat';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    template: `
      <div ejs-aiassistview 
        #aiAssistViewComponent
        (stopRespondingClick)="onStopResponding($event)">
      </div>
    `
})
export class AppComponent {
    private abortController: AbortController | null = null;
    
    public onStopResponding = (args: StopRespondingEventArgs) => {
        console.log('Stopping response for prompt:', args.prompt);
        console.log('Data index:', args.dataIndex);
        
        // Abort ongoing API request
        if (this.abortController) {
            this.abortController.abort();
            this.abortController = null;
        }
        
        // Update UI to show stopped state
        this.showStoppedMessage(args.dataIndex);
        
        // Log the action
        this.logStopAction(args.prompt, args.dataIndex);
    };
    
    private showStoppedMessage(index: number) {
        // Show partial response with indicator that it was stopped
        this.aiAssistViewComponent.addPromptResponse(
            'Response generation stopped by user.',
            index
        );
    }
    
    private logStopAction(prompt: string, index: number) {
        console.log(`User stopped response at index ${index} for prompt: "${prompt}"`);
    }
}
```

---

### AttachmentClickEventArgs

Provides information when an attached file is clicked.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `file` | FileInfo | Information about the clicked file |
| `event` | Event | Underlying DOM event |
| `cancel` | boolean | Set to true to cancel the default click action |
| `name` | string | Name of the event |

**Example Usage:**

```ts
import { AttachmentClickEventArgs } from '@syncfusion/ej2-angular-interactive-chat';

public onAttachmentClick = (args: AttachmentClickEventArgs) => {
    console.log('File clicked:', args.file.name);
    
    // Custom preview logic
    if (args.file.type.startsWith('image/')) {
        this.showImagePreview(args.file);
        args.cancel = true; // Prevent default action
    }
    
    // Download logic
    if (args.file.size > 10 * 1024 * 1024) { // 10MB
        if (!confirm('This file is large. Do you want to download it?')) {
            args.cancel = true;
        }
    }
};
```

---

### FileInfo Interface

Represents information about an attached file.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `name` | string | Name of the file |
| `size` | number | Size of the file in bytes |
| `type` | string | MIME type of the file |
| `rawFile` | File | Raw File object from the browser |

---

### Complete Event Handling Example

Here's a comprehensive example using all major event arguments:

```ts
import { Component, ViewChild } from '@angular/core';
import { 
  AIAssistViewModule, 
  AIAssistViewComponent,
  PromptRequestEventArgs,
  PromptChangedEventArgs,
  StopRespondingEventArgs,
  AttachmentClickEventArgs
} from '@syncfusion/ej2-angular-interactive-chat';

@Component({
  imports: [AIAssistViewModule],
  standalone: true,
  selector: 'app-root',
  template: `
    <div class="event-info" *ngIf="lastEvent">
      <strong>Last Event:</strong> {{ lastEvent }}
    </div>
    
    <div ejs-aiassistview 
      #aiAssistViewComponent
      [enableAttachments]="true"
      [attachmentSettings]="attachmentSettings"
      (promptRequest)="onPromptRequest($event)"
      (promptChanged)="onPromptChanged($event)"
      (stopRespondingClick)="onStopResponding($event)">
    </div>
  `
})
export class AppComponent {
  @ViewChild('aiAssistViewComponent')
  public aiAssistViewComponent!: AIAssistViewComponent;
  
  lastEvent = '';
  characterCount = 0;
  private abortController: AbortController | null = null;
  
  public attachmentSettings = {
    saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
    removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove',
    attachmentClick: this.onAttachmentClick.bind(this)
  };
  
  public onPromptRequest = (args: PromptRequestEventArgs) => {
    this.lastEvent = `promptRequest: "${args.prompt}"`;
    
    // Validation
    if (args.prompt.trim().length < 3) {
      args.cancel = true;
      alert('Prompt must be at least 3 characters');
      return;
    }
    
    // Handle attached files
    if (args.attachedFiles?.length > 0) {
      console.log(`Processing ${args.attachedFiles.length} files`);
    }
    
    // Create abort controller for cancellation
    this.abortController = new AbortController();
    
    // Call AI service
    this.processPrompt(args.prompt, this.abortController.signal);
  };
  
  public onPromptChanged = (args: PromptChangedEventArgs) => {
    this.lastEvent = `promptChanged: ${args.value.length} chars`;
    this.characterCount = args.value.length;
    
    // Validation feedback
    if (args.value.length > 1000) {
      console.warn('Prompt is getting long');
    }
  };
  
  public onStopResponding = (args: StopRespondingEventArgs) => {
    this.lastEvent = `stopResponding: index ${args.dataIndex}`;
    
    // Abort ongoing request
    if (this.abortController) {
      this.abortController.abort();
      this.abortController = null;
    }
    
    // Update UI
    this.aiAssistViewComponent.addPromptResponse(
      'Response stopped by user.',
      args.dataIndex
    );
  };
  
  public onAttachmentClick = (args: AttachmentClickEventArgs) => {
    this.lastEvent = `attachmentClick: ${args.file.name}`;
    
    // Custom file handling
    if (args.file.type.startsWith('image/')) {
      this.showImagePreview(args.file);
      args.cancel = true;
    }
  };
  
  private async processPrompt(prompt: string, signal: AbortSignal) {
    try {
      // Simulate AI processing
      await new Promise((resolve, reject) => {
        const timeout = setTimeout(resolve, 2000);
        signal.addEventListener('abort', () => {
          clearTimeout(timeout);
          reject(new Error('Aborted'));
        });
      });
      
      if (!signal.aborted) {
        this.aiAssistViewComponent.addPromptResponse(`Response to: ${prompt}`);
      }
    } catch (error: any) {
      if (error.message !== 'Aborted') {
        console.error('Error processing prompt:', error);
      }
    }
  }
  
  private showImagePreview(file: FileInfo) {
    // Image preview logic
    console.log('Showing preview for:', file.name);
  }
}
```

---


### AttachmentClick Event

The `attachmentClick` event is triggered when an attached file is clicked in the AI AssistView.

```ts
import { AIAssistViewModule, AttachmentClickEventArgs } from '@syncfusion/ej2-angular-interactive-chat';
import { Component, ViewChild } from '@angular/core';
import { AIAssistViewComponent } from '@syncfusion/ej2-angular-interactive-chat';

@Component({
    imports: [ AIAssistViewModule ],
    standalone: true,
    selector: 'app-root',
    // specifies the template string for the AI AssistView component
    template: `<div ejs-aiassistview #aiAssistViewComponent [enableAttachments]="enableAttachments" [attachmentSettings]="attachmentSettings" (promptRequest)="onPromptRequest()" (created)="onCreated()"  ></div>`
})

export class AppComponent {
    @ViewChild('aiAssistViewComponent')
    public aiAssistViewComponent!: AIAssistViewComponent;

    public onAttachmentClick = (args: AttachmentClickEventArgs) => {
        // Your required action here
    };
    public onPromptRequest = () => {
        setTimeout(() => {
          let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
          this.aiAssistViewComponent.addPromptResponse(defaultResponse);
        }, 1000);
    };
    public enableAttachments: boolean = true;
    public attachmentSettings = {
        saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
        removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove',
        attachmentClick: this.onAttachmentClick
    };
    public onCreated = () => {
        // Your required action here
    };
}
```

## Event Handling Best Practices

### 1. Always validate event data
- Check if files exist before processing in attachment events
- Validate prompt content before processing in promptRequest events
- Handle null or undefined values gracefully

### 2. Implement proper error handling
- Use try-catch blocks in event handlers
- Provide user feedback when errors occur
- Log errors for debugging purposes

### 3. Use arrow functions for event handlers
- Maintains proper `this` context in Angular components
- Ensures component methods can access component properties
- Recommended pattern for Syncfusion event handlers

### 4. Handle asynchronous operations
- Use async/await for cleaner code
- Always catch promise rejections
- Consider timeout handling for long-running operations

### 5. Manage file uploads properly
- Validate file types and sizes before upload
- Implement proper error handling for failed uploads
- Track upload progress for better UX

### Common Event Patterns

**Pattern 1: Validated Prompt Processing**
```ts
public onPromptRequest = () => {
    // Your required action here
    setTimeout(() => {
      let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
      this.aiAssistViewComponent.addPromptResponse(defaultResponse);
    }, 1000);
};
```

**Pattern 2: File Upload Validation**
```ts
public onBeforeAttachmentUpload = (args: UploadingEventArgs) => {
    // Your required action here
};
```

**Pattern 3: Post-Upload Processing**
```ts
public onAttachmentUploadSuccess = (args: UploadingEventArgs) => {
    // Your required action here
};
```

