# Serialization and Export

## Overview

**Serialization** saves diagram data to JSON. **Export** converts diagrams to images, PDF, or other formats.

---

## JSON Serialization

### Save Diagram to JSON

```typescript
// Save complete diagram
const diagramData = diagram.saveDiagram();
console.log(JSON.stringify(diagramData));

// Save to file
const blob = new Blob([JSON.stringify(diagramData)], { type: 'application/json' });
const url = URL.createObjectURL(blob);
const link = document.createElement('a');
link.href = url;
link.download = 'diagram.json';
link.click();
```

### Load Diagram from JSON

```typescript
// Load from variable
const diagramData = JSON.parse(jsonString);
diagram.loadDiagram(diagramData);

// Load from file
fileInput.addEventListener('change', (event) => {
  const file = event.target.files[0];
  const reader = new FileReader();
  reader.onload = (e) => {
    const data = JSON.parse(e.target.result);
    diagram.loadDiagram(data);
  };
  reader.readAsText(file);
});
```

### Serialized Data Structure

```json
{
  "nodes": [
    {
      "id": "node1",
      "offsetX": 100,
      "offsetY": 100,
      "width": 100,
      "height": 100,
      "shape": { "type": "Flow", "shape": "Process" }
    }
  ],
  "connectors": [
    {
      "id": "connector1",
      "sourceID": "node1",
      "targetID": "node2"
    }
  ]
}
```

### Selective Save

```typescript
// Save only nodes
const nodes = diagram.nodes;

// Save only connectors
const connectors = diagram.connectors;

// Save custom properties
const data = {
  nodes: diagram.nodes,
  connectors: diagram.connectors,
  customProperty: 'value'
};
```

---

## Image Export

### Export to PNG

```typescript
// Export entire diagram
diagram.exportDiagram({format: 'PNG', fileName: 'diagram'});

// Export selected area
diagram.exportDiagram({format: 'PNG', fileName: 'diagram', region: 'PageSettings'});
```

### Export to SVG

```typescript
diagram.exportDiagram({format: 'SVG', fileName: 'diagram'});
```

### Export to JPG

```typescript
diagram.exportDiagram({format: 'JPG', fileName: 'diagram'});
```

### Export Options

```typescript
const options = {
  type: 'PNG',          // PNG, SVG, JPG
  fileName: 'diagram',
  orientation: 'Portrait',  // Portrait, Landscape
  scale: 1.0,
  region: 'Content',    // Content, PageSettings
  multiplePage: false
};

diagram.exportDiagram(options);
```

### Export with Custom Size

```typescript
diagram.pageSettings = {
  width: 1920,
  height: 1080,
  orientation: 'Landscape'
};

diagram.exportDiagram({format: 'PNG', fileName: 'large-diagram.png'});
```

---

## Print Diagram

### Print to Printer

```typescript
diagram.print();
```

### Print with Options

```typescript
diagram.printSettings = {
  pageOrientation: 'Portrait',
  multiplePage: false,
  region: 'Content'
};

diagram.print(pageOrientation);
```

---

## Visio File Import

### Import Visio (.vsdx)

```typescript
import { BpmnDiagrams } from '@syncfusion/ej2-angular-diagrams';

@Component({
  viewProviders: [Inject(BpmnDiagrams)]
})
export class VisioImportComponent {
  importVisio = (file: File) => {
    const fileReader = new FileReader();
    fileReader.onload = (e: any) => {
      const data = e.target.result;
      // Parse and load Visio data
      diagram.loadDiagram(data);
    };
    fileReader.readAsArrayBuffer(file);
  };
}
```

---

## EJ1 Migration Serialization

### Migrate from EJ1 Format

```typescript
// EJ1 format
const ej1Data = {
  nodes: [
    {
      name: 'node1',
      offsetX: 100,
      offsetY: 100
    }
  ]
};

// Convert to EJ2 format
const ej2Data = {
  nodes: [
    {
      id: ej1Data.nodes[0].name,
      offsetX: ej1Data.nodes[0].offsetX,
      offsetY: ej1Data.nodes[0].offsetY
    }
  ]
};

diagram.loadDiagram(ej2Data);
```

### Key Property Changes

| EJ1 | EJ2 |
|-----|-----|
| `name` | `id` |
| N/A | `width`, `height` |
| `labels` | `annotations` |
| `fromNode` | `sourceID` |
| `toNode` | `targetID` |

---

## Custom Serialization

### Custom Save Handler

```typescript
const customSave = () => {
  const data = diagram.saveDiagram();
  
  // Add custom properties
  data.customMetadata = {
    version: '1.0',
    author: 'John Doe',
    created: new Date().toISOString()
  };
  
  return data;
};
```

### Custom Load Handler

```typescript
const customLoad = (jsonData) => {
  const data = JSON.parse(jsonData);
  
  // Read custom metadata
  console.log('Version:', data.customMetadata.version);
  
  // Load diagram
  diagram.loadDiagram(data);
};
```

---

**→ Next: Configure [diagram settings](diagram-settings.md) for appearance and behavior**
