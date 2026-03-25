# Data Structures in Syncfusion Angular File Manager

## Table of Contents
- [Overview](#overview)
- [Flat Data Structure](#flat-data-structure)
- [Nested Items](#nested-items)
- [fileData Interface](#filedata-interface)
- [Server-Based vs Local Data](#server-based-vs-local-data)
- [Hybrid Approaches](#hybrid-approaches)

## Overview

File Manager supports two data binding approaches:
1. **Server-Based** via `ajaxSettings` (backend handles file operations)
2. **Local Data** via `fileSystemData` (client-side JSON data)

## Flat Data Structure

### Flat Data Basics

Flat data uses a single-level array with parent-child relationships defined by `parentId`:

```typescript
const flatData = [
  { id: '1', name: 'Documents', isFile: false, parentId: null, hasChild: true },
  { id: '2', name: 'Music', isFile: false, parentId: null, hasChild: true },
  { id: '3', name: 'Work.pdf', isFile: true, parentId: '1', size: 102400 },
  { id: '4', name: 'Personal.txt', isFile: true, parentId: '1', size: 2048 },
  { id: '5', name: 'Rock', isFile: false, parentId: '2', hasChild: false },
  { id: '6', name: 'Song1.mp3', isFile: true, parentId: '5', size: 5242880 }
];
```

### Implementation

```typescript
import { Component } from '@angular/core';
import { FileManagerModule } from '@syncfusion/ej2-angular-filemanager';
import { FileData } from '@syncfusion/ej2-filemanager';

@Component({
  imports: [FileManagerModule],
  standalone: true,
  selector: 'app-root',
  template: `<ejs-filemanager 
    id='file-manager'
    [fileSystemData]='fileSystemData'
    height="500px">
  </ejs-filemanager>`
})
export class AppComponent {
  public fileSystemData: FileData[] = [
    {
      id: '1',
      name: 'Documents',
      isFile: false,
      parentId: null,
      hasChild: true,
      dateModified: new Date('2024-01-15')
    },
    {
      id: '2',
      name: 'Work.pdf',
      isFile: true,
      parentId: '1',
      hasChild: false,
      size: 102400,
      dateModified: new Date('2024-01-10')
    },
    {
      id: '3',
      name: 'Music',
      isFile: false,
      parentId: null,
      hasChild: true,
      dateModified: new Date('2024-01-20')
    }
  ];
}
```

### Advantages
- ✅ Simple, flat array structure
- ✅ Easy to manage from API responses
- ✅ Efficient for large datasets
- ✅ Database-friendly format

### Disadvantages
- ❌ Requires mapping parent-child relationships
- ❌ More complex to visualize hierarchy

## Nested Items

### Nested Data Basics

Nested data embeds children directly in parent objects using `children` array:

```typescript
const nestedData = [
  {
    id: '1',
    name: 'Documents',
    isFile: false,
    hasChild: true,
    children: [
      {
        id: '2',
        name: 'Work.pdf',
        isFile: true,
        size: 102400
      },
      {
        id: '3',
        name: 'Personal.txt',
        isFile: true,
        size: 2048
      }
    ]
  },
  {
    id: '4',
    name: 'Music',
    isFile: false,
    hasChild: true,
    children: [
      {
        id: '5',
        name: 'Song1.mp3',
        isFile: true,
        size: 5242880
      }
    ]
  }
];
```

### Implementation

```typescript
import { Component } from '@angular/core';
import { FileManagerModule } from '@syncfusion/ej2-angular-filemanager';

@Component({
  imports: [FileManagerModule],
  standalone: true,
  selector: 'app-root',
  template: `<ejs-filemanager 
    id='file-manager'
    [fileSystemData]='fileSystemData'
    height="500px">
  </ejs-filemanager>`
})
export class AppComponent {
  public fileSystemData: any[] = [
    {
      id: '1',
      name: 'Documents',
      isFile: false,
      hasChild: true,
      children: [
        {
          id: '2',
          name: 'Work.pdf',
          isFile: true,
          size: 102400,
          dateModified: new Date('2024-01-10')
        }
      ],
      dateModified: new Date('2024-01-15')
    }
  ];
}
```

### Advantages
- ✅ Natural hierarchy representation
- ✅ Easy to visualize folder structure
- ✅ Intuitive for nested folder exploration
- ✅ Self-contained tree structure

### Disadvantages
- ❌ Can become deeply nested and complex
- ❌ Less efficient for very large datasets
- ❌ Harder to serialize/deserialize

## fileData Interface

### Required Properties

```typescript
interface FileData {
  // Unique identifier
  id: string;
  
  // File or folder name
  name: string;
  
  // Whether this item is a file (true) or folder (false)
  isFile: boolean;
  
  // File size in bytes (required for files, 0 for folders)
  size?: number;
  
  // Last modification date
  dateModified?: Date;
  
  // Whether folder has children (required for folders)
  hasChild?: boolean;
  
  // For flat data: parent folder ID (null for root items)
  parentId?: string | null;
  
  // For nested data: child items
  children?: FileData[];
  
  // File MIME type (for files)
  type?: string;
  
  // Date file/folder was created
  dateCreated?: Date;
  
  // Filter path for organizing items hierarchically
  filterPath?: string;
  
  // File permissions and access control
  permission?: Permission;
}

interface Permission {
  copy: boolean;
  download: boolean;
  write: boolean;
  writeContents: boolean;
  read: boolean;
  upload: boolean;
  message?: string;
}
```

### Complete Example

```typescript
const completeFileData: FileData[] = [
  {
    id: '1',
    name: 'Documents',
    isFile: false,
    size: 0,
    dateModified: new Date('2024-01-15T10:30:00Z'),
    hasChild: true,
    parentId: null,
    type: null
  },
  {
    id: '2',
    name: 'Report.pdf',
    isFile: true,
    size: 204800,
    dateModified: new Date('2024-01-10T14:22:00Z'),
    hasChild: false,
    parentId: '1',
    type: 'application/pdf'
  },
  {
    id: '3',
    name: 'Notes.txt',
    isFile: true,
    size: 2048,
    dateModified: new Date('2024-01-12T09:15:00Z'),
    hasChild: false,
    parentId: '1',
    type: 'text/plain'
  }
];
```

## Server-Based vs Local Data

### Server-Based Data

**Use when:**
- Files are on a real file system
- Need dynamic file operations
- Backend handles file management
- Large, frequently changing datasets

```typescript
@Component({
  template: `<ejs-filemanager 
    [ajaxSettings]='ajaxSettings'>
  </ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: '/api/FileManager/FileOperations',
    uploadUrl: '/api/FileManager/Upload',
    downloadUrl: '/api/FileManager/Download'
  };
}
```

### Local Data

**Use when:**
- Mock/demo data
- Data from API needs client-side formatting
- Testing UI without backend
- Small, static datasets

```typescript
@Component({
  template: `<ejs-filemanager 
    [fileSystemData]='fileSystemData'>
  </ejs-filemanager>`
})
export class AppComponent {
  public fileSystemData: FileData[] = [
    { id: '1', name: 'Folder', isFile: false, parentId: null, hasChild: true, dateModified: new Date() }
  ];
}
```

## Hybrid Approaches

### Convert Flat to Nested

Transform flat data to nested structure:

```typescript
function flatToNested(flatData: FileData[]): FileData[] {
  const map: { [key: string]: FileData } = {};
  const roots: FileData[] = [];

  // Create map for quick lookup
  flatData.forEach(item => {
    map[item.id] = { ...item, children: [] };
  });

  // Assign children and collect roots
  flatData.forEach(item => {
    if (item.parentId === null || item.parentId === undefined) {
      roots.push(map[item.id]);
    } else {
      if (!map[item.parentId].children) {
        map[item.parentId].children = [];
      }
      map[item.parentId].children!.push(map[item.id]);
    }
  });

  return roots;
}
```

### Convert Nested to Flat

Transform nested data to flat structure:

```typescript
function nestedToFlat(nestedData: FileData[]): FileData[] {
  const flat: FileData[] = [];

  function traverse(items: FileData[], parentId: string | null = null) {
    items.forEach(item => {
      flat.push({
        ...item,
        parentId: parentId,
        children: undefined
      });
      
      if (item.children && item.children.length > 0) {
        traverse(item.children, item.id);
      }
    });
  }

  traverse(nestedData);
  return flat;
}
```

### Format API Response to fileData

Transform API response to File Manager format:

```typescript
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { FileData } from '@syncfusion/ej2-filemanager';

@Component({
  template: `<ejs-filemanager 
    [fileSystemData]='fileSystemData'>
  </ejs-filemanager>`
})
export class AppComponent implements OnInit {
  public fileSystemData: FileData[] = [];

  constructor(private http: HttpClient) {}

  ngOnInit() {
    // Fetch files from API
    this.http.get<any[]>('/api/files').subscribe(apiFiles => {
      // Transform API response to fileData format
      this.fileSystemData = apiFiles.map(file => ({
        id: file.fileId,
        name: file.fileName,
        isFile: file.type === 'file',
        size: file.fileSize,
        dateModified: new Date(file.lastModified),
        hasChild: file.type === 'folder' && file.childCount > 0,
        parentId: file.parentId || null,
        type: file.mimeType
      }));
    });
  }
}
```

### Dynamic Data Loading

Load data dynamically on folder open:

```typescript
import { Component, ViewChild } from '@angular/core';
import { FileManagerComponent, FileManagerModule } from '@syncfusion/ej2-angular-filemanager';
import { HttpClient } from '@angular/common/http';

@Component({
  imports: [FileManagerModule],
  template: `<ejs-filemanager 
    #fileManager
    [fileSystemData]='fileSystemData'
    (beforeOpen)='onBeforeOpen($event)'>
  </ejs-filemanager>`
})
export class AppComponent {
  @ViewChild('fileManager')
  public fileManager?: FileManagerComponent;

  public fileSystemData: any[] = [
    { id: '1', name: 'Folder', isFile: false, parentId: null, hasChild: true, dateModified: new Date() }
  ];

  constructor(private http: HttpClient) {}

  onBeforeOpen(args: any) {
    // Load children when folder is opened
    const folderId = args.fileDetails.id;
    
    this.http.get<any[]>(`/api/files/${folderId}`).subscribe(children => {
      // Add children to parent
      const parent = this.findFileById(folderId, this.fileSystemData);
      if (parent) {
        parent.children = children.map(file => ({
          id: file.fileId,
          name: file.fileName,
          isFile: file.type === 'file',
          size: file.fileSize,
          dateModified: new Date(file.lastModified),
          hasChild: file.type === 'folder',
          parentId: folderId
        }));
      }
    });
  }

  private findFileById(id: string, data: any[]): any {
    for (let item of data) {
      if (item.id === id) return item;
      if (item.children) {
        const found = this.findFileById(id, item.children);
        if (found) return found;
      }
    }
    return null;
  }
}
```

---

**Next:** Learn about drag-and-drop operations at [drag-and-drop.md](drag-and-drop.md) or explore large file handling at [virtualization.md](virtualization.md).
