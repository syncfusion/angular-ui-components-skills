# Complete API Reference for Syncfusion Angular File Manager

## Table of Contents
- [Component Properties](#component-properties)
- [Component Methods](#component-methods)
- [Component Events](#component-events)
- [FileData Interface](#filedata-interface)
- [Support Services](#support-services)

---

## Component Properties

### Core Configuration Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ajaxSettings` | AjaxSettingsModel | - | Specifies the AJAX settings for server-side file operations (URLs for file operations, upload, download, getImage) |
| `fileSystemData` | Object[] | - | Array of file/folder data for client-side binding (alternative to ajaxSettings) |
| `path` | string | `/` | Initial directory path to display |
| `view` | ViewType | `Details` | Initial view mode: 'Details' or 'LargeIcons' |
| `height` | string \| number | `100%` | Height of the file manager component |
| `width` | string \| number | `100%` | Width of the file manager component |
| `locale` | string | `en-US` | Localization culture (e.g., 'de-DE', 'ja-JP', 'es-ES') |
| `cssClass` | string | - | Custom CSS class for theming and styling |

### Selection and Multi-Selection

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `allowMultiSelection` | boolean | `true` | Enable multiple file/folder selection with Ctrl+Click or Shift+Click |
| `enableRangeSelection` | boolean | `false` | Allow drag-based multiple selection like Windows Explorer |
| `selectedItems` | string[] | [] | (Read-only) Array of currently selected file/folder IDs or names |

### File Operations

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `allowDragAndDrop` | boolean | `true` | Enable drag-and-drop file/folder movement |
| `uploadSettings` | UploadSettingsModel | - | Configuration for file upload (maxFileSize, minFileSize, autoUpload, allowedExtensions) |
| `searchSettings` | SearchSettingsModel | - | Search configuration (filterType, allowSearchOnTyping, caseSensitive) |

### Display and Behavior

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `showThumbnail` | boolean | `true` | Display thumbnails for image files |
| `enablePersistence` | boolean | `false` | Persist view, path, and selected items across page reload |
| `enableVirtualization` | boolean | `false` | Enable UI virtualization for large file sets (1000+ items) |
| `enableRtl` | boolean | `false` | Enable right-to-left layout for RTL languages |
| `enableHtmlSanitizer` | boolean | `true` | Sanitize HTML content for security (prevent XSS) |

### UI Customization

| Property | Type | Description |
|----------|------|-------------|
| `detailsViewSettings` | DetailsViewSettingsModel | Configure details view columns, sorting, gridlines |
| `navigationPaneSettings` | NavigationPaneSettingsModel | Configure left sidebar (folders/favorites) |
| `navigationPaneTemplate` | string \| object | Custom template for navigation pane nodes |
| `largeIconsTemplate` | string \| object | Custom template for large icons view items |
| `toolbarSettings` | ToolbarSettingsModel | Configure toolbar items and display |
| `contextMenuSettings` | ContextMenuSettingsModel | Configure context menu items |

### Sorting and Comparison

| Property | Type | Description |
|----------|------|-------------|
| `sortComparer` | Function | Custom comparison function for sorting files/folders (Windows Explorer-like sorting available) |

---

## Component Methods

### Navigation Methods

```typescript
// Navigate to a specific path
openFile(id: string): void

// Move back one level in the folder hierarchy
traverseBackward(): void

// Refresh the current folder contents
refreshFiles(): void

// Refresh the entire layout
refreshLayout(): void
```

### File/Folder Operations Methods

```typescript
// Create a new folder
createFolder(name?: string): void
// If name is provided, creates folder directly; otherwise opens new folder dialog

// Delete files/folders
deleteFiles(ids?: string[]): void
// If IDs provided, deletes those items; otherwise deletes selected items

// Rename a file/folder
renameFile(id?: string, name?: string): void
// If both parameters provided, renames directly; otherwise opens rename dialog

// Download files/folders
downloadFiles(ids?: string[]): void
// If IDs provided, downloads those items; otherwise downloads selected items

// Open upload dialog
uploadFiles(): void
```

### Selection Methods

```typescript
// Select all files/folders in current path
selectAll(): void

// Deselect all currently selected items
clearSelection(): void

// Get details of selected files
getSelectedFiles(): Object[]
```

### Toolbar and Menu Methods

```typescript
// Enable specific toolbar items
enableToolbarItems(items: string[]): void

// Disable specific toolbar items
disableToolbarItems(items: string[]): void

// Enable specific context menu items
enableMenuItems(items: string[]): void

// Disable specific context menu items
disableMenuItems(items: string[]): void

// Get index position of toolbar item
getToolbarItemIndex(item: string): number

// Get index position of menu item
getMenuItemIndex(item: string): number
```

### Dialog Methods

```typescript
// Programmatically close open dialogs
closeDialog(): void
```

### Filtering Methods

```typescript
// Display custom filtered files
filterFiles(filterData?: Object): void
// filterData should include custom action name and filter details
```

### Lifecycle Methods

```typescript
// Destroy the component
destroy(): void
```

---

## Component Events

### File Selection Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `fileSelect` | FileSelectEventArgs | Fires when a file/folder is selected or unselected |
| `fileSelection` | FileSelectionEventArgs | Fires **before** a file/folder is selected (can prevent selection) |

### File Opening Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `fileOpen` | FileOpenEventArgs | Fires **before** opening a file/folder (double-click or Enter) |
| `fileLoad` | FileLoadEventArgs | Fires **before** rendering each file/folder item |

### File Operation Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `beforeDelete` | DeleteEventArgs | Fires **before** deleting a file/folder (can prevent deletion) |
| `delete` | DeleteEventArgs | Fires **after** successful deletion |
| `beforeRename` | RenameEventArgs | Fires **before** renaming a file/folder (can prevent rename) |
| `beforeFolderCreate` | FolderCreateEventArgs | Fires **before** creating a folder (can prevent creation) |
| `folderCreate` | FolderCreateEventArgs | Fires **after** successfully creating a folder |
| `beforeMove` | MoveEventArgs | Fires **before** moving a file/folder (cut/paste) |

### Drag and Drop Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `fileDragStart` | FileDragEventArgs | Fires when drag operation starts |
| `fileDragging` | FileDragEventArgs | Fires **while** dragging files/folders |
| `fileDragStop` | FileDragEventArgs | Fires **before** dropping files/folders (can prevent drop) |
| `fileDropped` | FileDragEventArgs | Fires **after** dropping files/folders |

### Server Communication Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `beforeSend` | BeforeSendEventArgs | Fires **before** sending AJAX request (customize request params) |
| `success` | SuccessEventArgs | Fires **after** successful AJAX response |
| `failure` | FailureEventArgs | Fires when AJAX request fails |
| `beforeDownload` | BeforeDownloadEventArgs | Fires **before** download request to server |
| `beforeImageLoad` | BeforeImageLoadEventArgs | Fires **before** loading images from server |

### Dialog and Menu Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `beforePopupOpen` | BeforePopupOpenCloseEventArgs | Fires **before** opening dialogs (can prevent opening) |
| `popupOpen` | PopupOpenCloseEventArgs | Fires **after** opening dialogs (informational, cannot prevent) |
| `beforePopupClose` | BeforePopupOpenCloseEventArgs | Fires **before** closing dialogs (can prevent closing) |
| `popupClose` | PopupOpenCloseEventArgs | Fires **after** closing dialogs (informational) |
| `menuOpen` | MenuOpenEventArgs | Fires **before** opening context menu |
| `menuClose` | MenuCloseEventArgs | Fires **before** closing context menu |
| `menuClick` | MenuClickEventArgs | Fires when context menu item is clicked |
| `toolbarCreate` | ToolbarCreateEventArgs | Fires when toolbar is created (customize toolbar items) |
| `toolbarClick` | ToolbarClickEventArgs | Fires when toolbar item is clicked |

### Component Lifecycle Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `created` | Object | Fires when component is created and initialized |
| `destroyed` | Object | Fires when component is destroyed |

---

## FileData Interface

The `fileData` interface defines the structure for files and folders in client-side data binding.

### Required Properties

```typescript
interface FileData {
  // Unique identifier
  id: string | number;
  
  // File or folder name
  name: string;
  
  // Whether item is a file (true) or folder (false)
  isFile: boolean;
  
  // Parent folder ID (null/undefined for root items, use parentId for nested structure)
  parentId?: string | number | null;
}
```

### Optional Properties for Enhanced Display

```typescript
interface FileData {
  // File size in bytes
  size?: number;
  
  // Last modified date
  dateModified?: Date;
  
  // Date created
  dateCreated?: Date;
  
  // File type/extension
  type?: string;
  
  // Whether folder has child items
  hasChild?: boolean;
  
  // Custom image URL for thumbnail
  imageUrl?: string;
  
  // Filter path for traversal (internally managed)
  filterPath?: string;
  
  // Access control permissions
  permission?: {
    read?: boolean;
    readContent?: boolean;
    write?: boolean;
    writeContent?: boolean;
    delete?: boolean;
    download?: boolean;
    upload?: boolean;
    copy?: boolean;
  };
}
```

### Example FileData Structure

```typescript
// Flat structure with parentId
const flatData: FileData[] = [
  {
    id: '1',
    name: 'Documents',
    isFile: false,
    hasChild: true,
    dateModified: new Date('2024-01-15'),
    size: 0
  },
  {
    id: '2',
    name: 'Resume.pdf',
    isFile: true,
    parentId: '1',
    size: 204800,
    type: '.pdf',
    dateModified: new Date('2024-01-10')
  },
  {
    id: '3',
    name: 'Photo.jpg',
    isFile: true,
    parentId: '1',
    size: 2048000,
    type: '.jpg',
    imageUrl: '/assets/photo-thumb.jpg',
    dateModified: new Date('2024-01-05')
  }
];
```

---

## Support Services

### Module Imports and Providers

The following services should be provided for specific features:

```typescript
import { 
  FileManagerModule, 
  NavigationPaneService,
  DetailsViewService, 
  ToolbarService,
  VirtualizationService 
} from '@syncfusion/ej2-angular-filemanager';

@Component({
  imports: [FileManagerModule],
  providers: [
    NavigationPaneService,      // For left sidebar navigation
    DetailsViewService,         // For details view with sorting/columns
    ToolbarService,            // For toolbar functionality
    VirtualizationService      // For virtualization (large datasets)
  ]
})
export class FileManagerComponent { }
```

### Static Utility Functions

```typescript
import { FileManager } from '@syncfusion/ej2-filemanager';

// Check if fileSystemData is being used (not ajaxSettings)
const isLocalData = FileManager.isFileSystemData(fileManagerInstance);

// Sort comparison function (Windows Explorer-like natural sorting)
const sortResult = FileManager.sortComparer(
  'file1.txt',  // first string
  'file2.txt'   // second string
);

// Trigger fetch success event
FileManager.triggerFetchSuccess(instance, ajaxSettings);

// Trigger fetch failure event
FileManager.triggerFetchFailure(instance, ajaxSettings, errorResult);
```

---

## Configuration Models Reference

### AjaxSettingsModel

```typescript
ajaxSettings = {
  url: '/api/FileManager/FileOperations',
  getImageUrl: '/api/FileManager/GetImage',
  uploadUrl: '/api/FileManager/Upload',
  downloadUrl: '/api/FileManager/Download'
};
```

### UploadSettingsModel

```typescript
uploadSettings = {
  maxFileSize: 104857600,           // 100MB in bytes
  minFileSize: 0,
  autoUpload: true,
  allowedExtensions: ['.jpg', '.png', '.pdf'],
  directoryUpload: true            // Allow directory upload
};
```

### SearchSettingsModel

```typescript
searchSettings = {
  allowSearchOnTyping: true,
  caseSensitive: false,
  filterType: 'StartsWith'          // 'Contains', 'StartsWith', 'EndsWith'
};
```

### DetailsViewSettingsModel

```typescript
detailsViewSettings = {
  columns: [
    { field: 'name', headerText: 'Name', width: '30%' },
    { field: 'dateModified', headerText: 'Date Modified', width: '30%' },
    { field: 'type', headerText: 'Type', width: '15%' },
    { field: 'size', headerText: 'Size', width: '15%' }
  ]
};
```

### NavigationPaneSettingsModel

```typescript
navigationPaneSettings = {
  visible: true,
  minWidth: '150px',
  maxWidth: '500px'
};
```

### ToolbarSettingsModel

```typescript
toolbarSettings = {
  items: [
    'NewFolder', 
    'Upload', 
    'Delete', 
    'Download', 
    'Rename', 
    'SortBy', 
    'View', 
    'Refresh',
    'Cut', 
    'Copy', 
    'Paste',
    'Details'
  ]
};
```

### ContextMenuSettingsModel

```typescript
contextMenuSettings = {
  file: ['Open', 'Download', '|', 'Cut', 'Copy', 'Delete', '|', 'Rename'],
  folder: ['Open', 'Paste', '|', 'Cut', 'Copy', 'Delete', '|', 'Rename'],
  layout: ['Refresh', '|', 'Paste', '|', 'NewFolder', 'Upload']
};
```

---

## Best Practices for API Usage

### 1. Accessing Component Methods with ViewChild

```typescript
import { FileManagerComponent } from '@syncfusion/ej2-angular-filemanager';

@Component({
  selector: 'app-root',
  template: `
    <ejs-filemanager #fileManager [ajaxSettings]="ajaxSettings">
    </ejs-filemanager>
  `
})
export class AppComponent implements AfterViewInit {
  @ViewChild('fileManager')
  fileManager?: FileManagerComponent;

  ngAfterViewInit() {
    // Now you can call methods
    this.fileManager?.selectAll();
  }
}
```

### 2. Handling Events Properly

```typescript
onFileSelect(args: FileSelectEventArgs) {
  console.log('Selected files:', args.fileDetails);
  console.log('Selection type:', args.isSelectAll);
}

onBeforeDelete(args: DeleteEventArgs) {
  // Prevent deletion of system files
  if (args.fileDetails[0].name.startsWith('.')) {
    args.cancel = true;  // Prevent deletion
  }
}
```

### 3. Programmatic File Operations

```typescript
// Delete selected files with confirmation
deleteSelected() {
  const selected = this.fileManager?.selectedItems;
  if (selected && selected.length > 0) {
    if (confirm(`Delete ${selected.length} items?`)) {
      this.fileManager?.deleteFiles();
    }
  }
}
```

### 4. Custom Sorting Implementation

```typescript
import { FileManager } from '@syncfusion/ej2-filemanager';

sortComparer = (a: string, b: string): number => {
  return FileManager.sortComparer(a, b);  // Windows Explorer-like sorting
}
```

