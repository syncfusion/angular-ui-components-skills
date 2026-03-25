# Interactive Chart Features

Interactive features enhance user engagement and data exploration. This guide covers zooming, panning, tooltips, crosshair, trackball, selection, data editing, and synchronized charts.

## Table of Contents

- [Zooming](#zooming)
- [Panning](#panning)
- [Tooltip](#tooltip)
- [Crosshair](#crosshair)
- [Trackball](#trackball)
- [Selection](#selection)
- [Data Editing](#data-editing)
- [Synchronized Charts](#synchronized-charts)

## Zooming

Zooming allows users to focus on specific data regions for detailed analysis.

**API Reference:**
- [ZoomSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettingsModel) - Complete zoom configuration
- [ZoomSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettings) - Zoom settings class
- [ZoomMode](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomMode) - Zoom mode enum (X, Y, XY)
- [ToolbarItems](https://ej2.syncfusion.com/angular/documentation/api/chart/toolbarItems) - Zoom toolbar items enum
- [IZoomingEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iZoomingEventArgs) - Zooming event interface
- [IZoomCompleteEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iZoomCompleteEventArgs) - Zoom complete event interface

**Key Events:**
- [ChartModel.onZooming](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#onZooming) - Triggered during zoom
- [ChartModel.zoomComplete](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#zoomComplete) - Triggered after zoom completes

### Enable Zooming

**API Properties:**
- [ZoomSettings.enableSelectionZooming](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettings#enableSelectionZooming) (boolean, default: false)
- [ZoomSettings.enableMouseWheelZooming](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettings#enableMouseWheelZooming) (boolean, default: false)
- [ZoomSettings.enablePinchZooming](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettings#enablePinchZooming) (boolean, default: false)
- [ZoomSettings.enableScrollbar](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettings#enableScrollbar) (boolean, default: false)
- [ZoomSettings.mode](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettings#mode) - [ZoomMode](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomMode) enum (default: 'XY')

```typescript
import { Component } from '@angular/core';
import { ChartModule, ZoomService } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-chart',
  standalone: true,
  imports: [ChartModule],
  providers: [ZoomService],  // Required
  template: `
    <ejs-chart [zoomSettings]="zoomSettings">
      <e-series-collection>
        <e-series [dataSource]="data" type="Line" xName="x" yName="y"></e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class ChartComponent {
  public zoomSettings = {
    enableSelectionZooming: true,
    enableMouseWheelZooming: true,
    enablePinchZooming: true,
    enableScrollbar: false
  };
  
  public data = [/* data */];
}
```

### Zoom Types

**Selection Zooming:**
```typescript
public zoomSettings = {
  enableSelectionZooming: true,  // Drag to select region
  mode: 'XY'  // X, Y, or XY
};
```

**Mouse Wheel Zooming:**
```typescript
public zoomSettings = {
  enableMouseWheelZooming: true  // Scroll to zoom
};
```

**Pinch Zooming:**
```typescript
public zoomSettings = {
  enablePinchZooming: true  // Touch pinch gesture
};
```

### Zoom Toolbar

Display toolbar with zoom controls.

```typescript
public zoomSettings = {
  enableSelectionZooming: true,
  mode: 'XY',
  toolbarItems: ['Zoom', 'ZoomIn', 'ZoomOut', 'Pan', 'Reset']
};
```

**Toolbar Items:**
- `Zoom`: Enable selection zoom
- `ZoomIn`: Zoom in incrementally
- `ZoomOut`: Zoom out incrementally
- `Pan`: Enable panning mode
- `Reset`: Reset to original view

### Zoom Mode

Control which axes can be zoomed.

```typescript
public zoomSettings = {
  enableSelectionZooming: true,
  mode: 'X'  // X, Y, or XY
};
```

- `X`: Zoom only horizontally
- `Y`: Zoom only vertically
- `XY`: Zoom both directions (default)

### Zoom with Scrollbar

Add scrollbar for navigating zoomed area.

```typescript
public zoomSettings = {
  enableSelectionZooming: true,
  enableScrollbar: true
};
```

### Programmatic Zoom

```typescript
import { ViewChild } from '@angular/core';
import { ChartComponent as SyncfusionChart } from '@syncfusion/ej2-angular-charts';

export class AppComponent {
  @ViewChild('chart') public chart: SyncfusionChart;
  
  public zoomIn() {
    this.chart.zoomModule.zoomIn();
  }
  
  public zoomOut() {
    this.chart.zoomModule.zoomOut();
  }
  
  public zoomByRange(start: number, end: number) {
    this.chart.zoomModule.zoom(start, end);
  }
  
  public resetZoom() {
    this.chart.zoomModule.reset();
  }
}
```

```html
<ejs-chart #chart [zoomSettings]="zoomSettings">
  <!-- series -->
</ejs-chart>
<button (click)="zoomIn()">Zoom In</button>
<button (click)="zoomOut()">Zoom Out</button>
<button (click)="resetZoom()">Reset</button>
```

## Panning

Move the view within a zoomed chart.

### Enable Panning

```typescript
public zoomSettings = {
  enableSelectionZooming: true,
  enablePan: true  // Enable panning after zoom
};
```

**Usage:**
1. Zoom into chart
2. Click and drag to pan
3. Or use Pan button in toolbar

### Pan Mode

```typescript
public zoomSettings = {
  enablePan: true,
  mode: 'X'  // Pan only horizontally
};
```

## Tooltip

Tooltips display data point information on hover or touch.

### Basic Tooltip

```typescript
import { TooltipService } from '@syncfusion/ej2-angular-charts';

@Component({
  providers: [TooltipService],  // Required
  // ...
})
export class ChartComponent {
  public tooltip = {
    enable: true
  };
}
```

```html
<ejs-chart [tooltip]="tooltip">
  <e-series-collection>
    <e-series [dataSource]="data" type="Line" xName="x" yName="y" name="Sales"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Tooltip Formatting

```typescript
public tooltip = {
  enable: true,
  format: '${series.name}: ${point.y}K',  // Custom format
  header: 'Month: ${point.x}'  // Tooltip header
};
```

**Format Placeholders:**
- `${series.name}`: Series name
- `${point.x}`: X value
- `${point.y}`: Y value
- `${point.percentage}`: Percentage (pie charts)

### Tooltip Styling

```typescript
public tooltip = {
  enable: true,
  fill: '#333',
  opacity: 0.9,
  textStyle: {
    color: 'white',
    size: '14px',
    fontWeight: 'Bold'
  },
  border: {
    width: 2,
    color: '#FF5733'
  }
};
```

### Shared Tooltip

Show data from all series at the same x-position.

```typescript
public tooltip = {
  enable: true,
  shared: true  // Show all series values
};
```

### Custom Tooltip Template

```html
<ejs-chart [tooltip]="tooltip">
  <ng-template #tooltipTemplate let-data>
    <div style="background: white; padding: 10px; border: 2px solid #ccc; border-radius: 5px;">
      <b>{{data.x}}</b><br/>
      Sales: ${{data.y}}K<br/>
      <span [style.color]="data.point.color">●</span> {{data.series.name}}
    </div>
  </ng-template>
  <e-series-collection>
    <!-- series -->
  </e-series-collection>
</ejs-chart>
```

### Tooltip Events

```typescript
public tooltipRender(args: any) {
  // Customize tooltip before rendering
  args.text = `Custom: ${args.data.pointY}`;
}
```

```html
<ejs-chart [tooltip]="tooltip" (tooltipRender)="tooltipRender($event)">
  <!-- series -->
</ejs-chart>
```

## Crosshair

Crosshair shows vertical and horizontal lines at cursor position for precise value reading.

### Basic Crosshair

```typescript
import { CrosshairService } from '@syncfusion/ej2-angular-charts';

@Component({
  providers: [CrosshairService],  // Required
  // ...
})
export class ChartComponent {
  public crosshair = {
    enable: true
  };
}
```

```html
<ejs-chart [crosshair]="crosshair">
  <e-series-collection>
    <e-series [dataSource]="data" type="Line" xName="x" yName="y"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Crosshair Line Style

```typescript
public crosshair = {
  enable: true,
  lineType: 'Both',  // Vertical, Horizontal, Both
  line: {
    width: 2,
    color: 'red',
    dashArray: '5,5'
  }
};
```

### Crosshair Label

```typescript
public crosshair = {
  enable: true,
  lineType: 'Both'
};

public primaryXAxis = {
  crosshairTooltip: {
    enable: true,
    fill: '#FF5733',
    textStyle: { color: 'white' }
  }
};

public primaryYAxis = {
  crosshairTooltip: {
    enable: true,
    fill: '#3498db',
    textStyle: { color: 'white' }
  }
};
```

## Trackball

Trackball highlights the nearest data point with tooltip as cursor moves.

### Basic Trackball

```typescript
import { TooltipService } from '@syncfusion/ej2-angular-charts';

@Component({
  providers: [TooltipService],
  // ...
})
export class ChartComponent {
  public tooltip = {
    enable: true,
    shared: true  // Required for trackball
  };
  
  public crosshair = {
    enable: true,
    lineType: 'Vertical'
  };
}
```

```html
<ejs-chart [tooltip]="tooltip" [crosshair]="crosshair">
  <e-series-collection>
    <e-series [dataSource]="data1" type="Line" xName="x" yName="y" name="Series 1" [marker]="marker"></e-series>
    <e-series [dataSource]="data2" type="Line" xName="x" yName="y" name="Series 2" [marker]="marker"></e-series>
  </e-series-collection>
</ejs-chart>
```

**Trackball shows:**
- Tooltip with all series values at x-position
- Marker on each series at nearest point
- Vertical crosshair line

## Selection

Select data points, series, or regions for highlighting or further actions.

### Point Selection

```typescript
import { SelectionService } from '@syncfusion/ej2-angular-charts';

@Component({
  providers: [SelectionService],  // Required
  // ...
})
export class ChartComponent {
  public selectionMode = 'Point';  // Point, Series, Cluster, DragXY, DragX, DragY
  
  public selectionSettings = {
    enable: true,
    mode: 'Point'
  };
}
```

```html
<ejs-chart [selectionMode]="selectionMode" (chartMouseClick)="onChartClick($event)">
  <e-series-collection>
    <e-series [dataSource]="data" type="Column" xName="x" yName="y"></e-series>
  </e-series-collection>
</ejs-chart>
```

### Selection Modes

**Point:** Select individual data points
```typescript
public selectionMode = 'Point';
```

**Series:** Select entire series
```typescript
public selectionMode = 'Series';
```

**Cluster:** Select all points at same x-value (column charts)
```typescript
public selectionMode = 'Cluster';
```

**Drag Selection:** Select region by dragging
```typescript
public selectionMode = 'DragXY';  // DragX, DragY, DragXY
```

### Selection Styling

```typescript
public selectionSettings = {
  enable: true,
  mode: 'Point',
  pattern: 'DiagonalForward'  // None, Dots, DiagonalForward, DiagonalBackward, etc.
};
```

**Selection Patterns:**
- None (solid fill)
- Dots
- DiagonalForward
- Cross
- HorizontalDash
- VerticalDash
- Rectangle
- Box
- VerticalStripe
- HorizontalStripe

### Programmatic Selection

```typescript
import { ViewChild } from '@angular/core';
import { ChartComponent as SyncfusionChart } from '@syncfusion/ej2-angular-charts';

export class AppComponent {
  @ViewChild('chart') public chart: SyncfusionChart;
  
  public selectDataPoint(seriesIndex: number, pointIndex: number) {
    this.chart.selectedDataIndexes = [{ series: seriesIndex, point: pointIndex }];
    this.chart.dataBind();
  }
}
```

### Selection Events

```typescript
public onChartClick(args: any) {
  console.log('Selected:', args.selectedDataValues);
}

public onSelectionComplete(args: any) {
  console.log('Selection completed:', args);
}
```

```html
<ejs-chart (chartMouseClick)="onChartClick($event)" 
           (selectionComplete)="onSelectionComplete($event)">
  <!-- series -->
</ejs-chart>
```

## Data Editing

Edit data by dragging points (line/scatter charts).

### Enable Data Editing

```typescript
import { DataEditingService } from '@syncfusion/ej2-angular-charts';

@Component({
  providers: [DataEditingService],  // Required
  // ...
})
export class ChartComponent {
  public data = [
    { x: 'Jan', y: 35 },
    { x: 'Feb', y: 28 },
    { x: 'Mar', y: 34 }
  ];
}
```

```html
<ejs-chart>
  <e-series-collection>
    <e-series [dataSource]="data" type="Line" xName="x" yName="y"
              [dragSettings]="{ enable: true }" [marker]="{ visible: true, width: 10, height: 10 }">
    </e-series>
  </e-series-collection>
</ejs-chart>
```

### Drag Settings

```typescript
public dragSettings = {
  enable: true,
  minY: 0,  // Minimum Y value
  maxY: 100  // Maximum Y value
};
```

### Drag Events

```typescript
public onDragStart(args: any) {
  console.log('Drag started:', args.data);
}

public onDragEnd(args: any) {
  console.log('New value:', args.data.y);
  // Update backend/state with new value
}
```

```html
<e-series [dragSettings]="dragSettings" 
          (dragStart)="onDragStart($event)"
          (dragEnd)="onDragEnd($event)">
</e-series>
```

## Synchronized Charts

Link multiple charts so interactions (zoom, tooltip, crosshair) affect all.

### Setup Synchronized Charts

```typescript
import { Component, ViewChild } from '@angular/core';
import { ChartComponent as SyncfusionChart } from '@syncfusion/ej2-angular-charts';

@Component({
  selector: 'app-sync-charts',
  template: `
    <ejs-chart #chart1 [primaryXAxis]="primaryXAxis" [zoomSettings]="zoomSettings"
               (chartMouseMove)="onMouseMove($event, 1)"
               (zoomComplete)="onZoomComplete($event)">
      <e-series-collection>
        <e-series [dataSource]="data1" type="Line" xName="x" yName="y"></e-series>
      </e-series-collection>
    </ejs-chart>
    
    <ejs-chart #chart2 [primaryXAxis]="primaryXAxis" [zoomSettings]="zoomSettings"
               (chartMouseMove)="onMouseMove($event, 2)"
               (zoomComplete)="onZoomComplete($event)">
      <e-series-collection>
        <e-series [dataSource]="data2" type="Line" xName="x" yName="y"></e-series>
      </e-series-collection>
    </ejs-chart>
  `
})
export class SyncChartsComponent {
  @ViewChild('chart1') public chart1: SyncfusionChart;
  @ViewChild('chart2') public chart2: SyncfusionChart;
  
  public primaryXAxis = { valueType: 'Category' };
  public zoomSettings = { enableSelectionZooming: true, mode: 'X' };
  
  public data1 = [/* data */];
  public data2 = [/* data */];
  
  public onMouseMove(args: any, chartId: number) {
    // Sync crosshair position
    if (chartId === 1 && this.chart2) {
      // Update chart2 crosshair
    } else if (chartId === 2 && this.chart1) {
      // Update chart1 crosshair
    }
  }
  
  public onZoomComplete(args: any) {
    const zoomFactor = args.currentZoomFactor;
    const zoomPosition = args.currentZoomPosition;
    
    // Apply same zoom to both charts
    if (this.chart1) {
      this.chart1.zoomModule.zoom(zoomFactor, zoomPosition);
    }
    if (this.chart2) {
      this.chart2.zoomModule.zoom(zoomFactor, zoomPosition);
    }
  }
}
```

### Sync Crosshair

```typescript
public onMouseMove(args: any) {
  const charts = [this.chart1, this.chart2, this.chart3];
  
  charts.forEach(chart => {
    if (chart && chart.crosshairModule) {
      chart.crosshairModule.show(args.x, args.y);
    }
  });
}
```

### Sync Tooltip

```typescript
public onMouseMove(args: any) {
  const charts = [this.chart1, this.chart2];
  
  charts.forEach(chart => {
    if (chart && chart.tooltipModule) {
      chart.tooltipModule.show(args.x, args.y);
    }
  });
}
```

## Best Practices

### Zooming
- Enable selection + mouse wheel for flexibility
- Show zoom toolbar for user guidance
- Add scrollbar for long time-series data

### Tooltip
- Use shared tooltip for multi-series charts
- Format values for readability ($, %, K, M)
- Keep tooltip content concise

### Crosshair
- Combine with tooltip for maximum info
- Use trackball for multi-series comparison
- Style crosshair lines to stand out

### Selection
- Provide visual feedback (patterns, colors)
- Handle selection events to take action
- Clear instructions for drag selection

### Synchronized Charts
- Ensure same x-axis scale/range
- Sync only related interactions
- Test performance with many charts

## Common Pitfalls

1. **Missing Service Providers:** Forgetting to inject required services (ZoomService, TooltipService, etc.)
2. **Performance Issues:** Too many interactions on large datasets
3. **Conflicting Features:** Zoom + drag selection can interfere
4. **Poor UX:** No instructions for users on how to interact
5. **Tooltip Overlap:** Multiple tooltips competing for space

## Performance Tips

- Disable animations during zooming: `animation: { enable: false }`
- Use canvas rendering for large datasets: `enableCanvas: true`
- Debounce mouse events in synchronized charts
- Limit tooltip updates to necessary data only

Refer to the advanced-features reference for event handling and API methods.

## API Reference Summary

### Zoom Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| ZoomSettingsModel | Complete zoom configuration interface | [zoomSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettingsModel) |
| ZoomSettings | Zoom settings class | [zoomSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettings) |
| ZoomMode | Zoom mode enum (X, Y, XY) | [zoomMode.md](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomMode) |
| ToolbarItems | Toolbar items enum | [toolbarItems.md](https://ej2.syncfusion.com/angular/documentation/api/chart/toolbarItems) |
| ScrollbarSettings | Scrollbar configuration | [scrollbarSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/scrollbarSettings) |
| ScrollbarSettingsModel | Scrollbar model interface | [scrollbarSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/scrollbarSettingsModel) |

### Tooltip Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| TooltipSettingsModel | Complete tooltip configuration interface | [tooltipSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipSettingsModel) |
| TooltipSettings | Tooltip settings class | [tooltipSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipSettings) |
| TooltipPosition | Tooltip position enum | [tooltipPosition.md](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipPosition) |
| FadeOutMode | Tooltip fade-out behavior | [fadeOutMode.md](https://ej2.syncfusion.com/angular/documentation/api/chart/fadeOutMode) |

### Crosshair Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| CrosshairSettingsModel | Crosshair configuration interface | [crosshairSettingsModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshairSettingsModel) |
| CrosshairSettings | Crosshair settings class | [crosshairSettings.md](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshairSettings) |
| CrosshairTooltipModel | Crosshair tooltip configuration | [crosshairTooltipModel.md](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshairTooltipModel) |

### Selection Configuration

| API | Description | Documentation |
|-----|-------------|---------------|
| SelectionMode | Selection mode enum (Point, Series, Cluster, DragXY, etc.) | [selectionMode.md](https://ej2.syncfusion.com/angular/documentation/api/chart/selectionMode) |
| SelectionPattern | Selection pattern enum (None, Dots, DiagonalForward, etc.) | [selectionPattern.md](https://ej2.syncfusion.com/angular/documentation/api/chart/selectionPattern) |
| HighlightMode | Highlight mode enum (None, Point, Series, etc.) | [highlightMode.md](https://ej2.syncfusion.com/angular/documentation/api/chart/highlightMode) |

### Key Properties Reference

| Feature | Important Properties | API Reference |
|---------|---------------------|---------------|
| **Zooming** | enableSelectionZooming, enableMouseWheelZooming, mode, toolbarItems | [ZoomSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettingsModel) |
| **Panning** | enablePan (via ZoomSettings) | [ZoomSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettingsModel) |
| **Tooltip** | enable, format, shared, fill, border, template | [TooltipSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipSettingsModel) |
| **Crosshair** | enable, line, lineType, dashArray | [CrosshairSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshairSettingsModel) |
| **Selection** | selectionMode, selectionPattern, highlightMode | [ChartModel](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel) |
| **Data Editing** | allowDataEdit (via Series) | [Series](https://ej2.syncfusion.com/angular/documentation/api/chart/series) |

### Events for Interactive Features

| Event | Interface | Description | API Reference |
|-------|-----------|-------------|---------------|
| onZooming | IZoomingEventArgs | Triggered during zoom operation | [ChartModel.onZooming](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#onZooming), [IZoomingEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iZoomingEventArgs) |
| zoomComplete | IZoomCompleteEventArgs | Triggered after zoom completes | [ChartModel.zoomComplete](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#zoomComplete), [IZoomCompleteEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iZoomCompleteEventArgs) |
| tooltipRender | ITooltipRenderEventArgs | Before tooltip rendering | [ChartModel.tooltipRender](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#tooltipRender), [ITooltipRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iTooltipRenderEventArgs) |
| sharedTooltipRender | ISharedTooltipRenderEventArgs | Before shared tooltip rendering | [ChartModel.sharedTooltipRender](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#sharedTooltipRender), [ISharedTooltipRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iSharedTooltipRenderEventArgs) |
| pointClick | IPointEventArgs | Point click event | [ChartModel.pointClick](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#pointClick), [IPointEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPointEventArgs) |
| pointDoubleClick | IPointEventArgs | Point double click | [ChartModel.pointDoubleClick](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#pointDoubleClick), [IPointEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPointEventArgs) |
| pointMove | IPointEventArgs | Mouse move over point | [ChartModel.pointMove](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#pointMove), [IPointEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPointEventArgs) |
| chartMouseClick | IMouseEventArgs | Chart mouse click | [ChartModel.chartMouseClick](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#chartMouseClick), [IMouseEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iMouseEventArgs) |
| chartMouseMove | IMouseEventArgs | Mouse move on chart | [ChartModel.chartMouseMove](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#chartMouseMove), [IMouseEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iMouseEventArgs) |
| chartMouseDown | IMouseEventArgs | Mouse down on chart | [ChartModel.chartMouseDown](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#chartMouseDown), [IMouseEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iMouseEventArgs) |
| chartMouseUp | IMouseEventArgs | Mouse up on chart | [ChartModel.chartMouseUp](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#chartMouseUp), [IMouseEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iMouseEventArgs) |
| selectionComplete | ISelectionCompleteEventArgs | After selection completes | [ChartModel.selectionComplete](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#selectionComplete), [ISelectionCompleteEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iSelectionCompleteEventArgs) |
| drag | IDragCompleteEventArgs | During drag operation | [ChartModel.drag](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#drag) |
| dragComplete | IDragCompleteEventArgs | After drag completes | [ChartModel.dragComplete](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#dragComplete), [IDragCompleteEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iDragCompleteEventArgs) |
| scrollStart | IScrollEventArgs | Scrollbar scroll start | [ChartModel.scrollStart](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#scrollStart), [IScrollEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iScrollEventArgs) |
| scrollEnd | IScrollEventArgs | Scrollbar scroll end | [ChartModel.scrollEnd](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#scrollEnd), [IScrollEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iScrollEventArgs) |
| scrollChanged | IScrollEventArgs | Scrollbar scroll change | [ChartModel.scrollChanged](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel#scrollChanged), [IScrollEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iScrollEventArgs) |

### Required Services

| Feature | Service | Import |
|---------|---------|--------|
| Zooming, Panning | ZoomService | @syncfusion/ej2-angular-charts |
| Tooltip | TooltipService | @syncfusion/ej2-angular-charts |
| Crosshair | CrosshairService | @syncfusion/ej2-angular-charts |
| Selection | SelectionService | @syncfusion/ej2-angular-charts |
| Data Editing | DataEditingService | @syncfusion/ej2-angular-charts |

**Note:** When using `ChartAllModule`, all services are automatically provided.
