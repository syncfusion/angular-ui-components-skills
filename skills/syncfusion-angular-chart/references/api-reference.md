# Syncfusion Angular Chart API Reference Guide

This document provides a comprehensive overview of the Syncfusion Angular Chart API documentation. All API documentation is available online at the official Syncfusion documentation site.

**Base URL:** https://ej2.syncfusion.com/angular/documentation/api/chart/

## API Documentation Overview

**Total APIs:** 200+ interfaces, classes, enums, and event interfaces  
**Categories:**
- **Interfaces/Models:** 100+ configuration interfaces
- **Classes:** 30+ implementation classes
- **Enumerations:** 50+ enum types
- **Event Interfaces:** 40+ event argument interfaces
- **Utility Types:** 20+ helper types

## Core Configuration APIs

### Primary Interfaces

| API | Properties | Description | Documentation Link |
|-----|------------|-------------|-------------------|
| **Chart** | 40+ | Main chart component with title, width, height, axes, series, theme, tooltip, legend, zoom, and all 40+ events | [Chart API](https://ej2.syncfusion.com/angular/documentation/api/chart/chart) |
| **Axis** | 50+ | Complete axis configuration: valueType, minimum, maximum, interval, labels, gridlines, ticks, crossing, formatting | [Axis API](https://ej2.syncfusion.com/angular/documentation/api/chart/axis) |
| **AxisModel** | 50+ | Axis model interface | [AxisModel API](https://ej2.syncfusion.com/angular/documentation/api/chart/axisModel) |
| **Series** | 60+ | Series configuration: type, dataSource, xName, yName, marker, animation, colors, borders, financial fields | [Series API](https://ej2.syncfusion.com/angular/documentation/api/chart/series) |
| **SeriesModel** | - | Series model interface | [SeriesModel API](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesModel) |

## Chart Elements APIs

### Legend

| API | Description | Documentation Link |
|-----|-------------|-------------------|
| LegendSettingsModel | Complete legend configuration interface | [LegendSettingsModel API](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettingsModel) |
| LegendSettings | Legend settings class | [LegendSettings API](https://ej2.syncfusion.com/angular/documentation/api/chart/legendSettings) |
| LegendPosition | Position enum (Top, Bottom, Left, Right, Custom) | [LegendPosition API](https://ej2.syncfusion.com/angular/documentation/api/chart/legendPosition) |
| LegendShape | Icon shape enum (Circle, Rectangle, Triangle, Diamond, etc.) | [LegendShape API](https://ej2.syncfusion.com/angular/documentation/api/chart/legendShape) |
| LegendMode | Rendering mode (Series, Point, Range, Gradient) | [LegendMode API](https://ej2.syncfusion.com/angular/documentation/api/chart/#legendmode) |

### Markers

| API | Description | Documentation Link |
|-----|-------------|----------|
| MarkerSettingsModel | Complete marker configuration interface | [markerSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettingsModel) |
| MarkerSettings | Marker settings class | [markerSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/markerSettings) |
| ChartShape | Marker shape enum (Circle, Rectangle, Diamond, Triangle, Pentagon, Cross, Plus, etc.) | [chartShape](https://ej2.syncfusion.com/angular/documentation/api/chart/chartShape) |

### Data Labels

| API | Description | Documentation Link |
|-----|-------------|----------|
| DataLabelSettingsModel | Data label configuration interface | [dataLabelSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelSettingsModel) |
| DataLabelSettings | Data label settings class | [dataLabelSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelSettings) |
| DataLabelIntersectAction | Overlap handling (None, Hide, Rotate, etc.) | [dataLabelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/dataLabelIntersectAction) |
| LabelPosition | Label position enum | [labelPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/labelPosition) |
| Alignment | Text alignment enum | [alignment](https://ej2.syncfusion.com/angular/documentation/api/chart/alignment) |
| TextAlignment | Text alignment options | [textAlignment](https://ej2.syncfusion.com/angular/documentation/api/chart/textAlignment) |
| TextOverflow | Text overflow behavior | [textOverflow](https://ej2.syncfusion.com/angular/documentation/api/chart/textOverflow) |
| TextWrap | Text wrapping options | [textWrap](https://ej2.syncfusion.com/angular/documentation/api/chart/textWrap) |

### Tooltip

| API | Description | Documentation Link |
|-----|-------------|----------|
| TooltipSettingsModel | Complete tooltip configuration interface | [tooltipSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipSettingsModel) |
| TooltipSettings | Tooltip settings class | [tooltipSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipSettings) |
| TooltipPosition | Tooltip position enum | [tooltipPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/tooltipPosition) |
| FadeOutMode | Tooltip fade-out behavior | [fadeOutMode](https://ej2.syncfusion.com/angular/documentation/api/chart/fadeOutMode) |

### Annotations

| API | Description | Documentation Link |
|-----|-------------|----------|
| ChartAnnotationSettingsModel | Annotation configuration interface | [chartAnnotationSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/chartAnnotationSettingsModel) |
| ChartAnnotationSettings | Annotation settings class | [chartAnnotationSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/chartAnnotationSettings) |
| AnnotationDirective | Annotation directive for declaring annotations | [annotationDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/annotationDirective) |
| Anchor | Anchor point enum (Chart, Series, Point) | [anchor](https://ej2.syncfusion.com/angular/documentation/api/chart/anchor) |
| Position | Positioning enum | [position](https://ej2.syncfusion.com/angular/documentation/api/chart/position) |

### Trendlines

| API | Description | Documentation Link |
|-----|-------------|----------|
| TrendlineModel | Trendline configuration interface | [trendlineModel](https://ej2.syncfusion.com/angular/documentation/api/chart/trendlineModel) |
| Trendline | Trendline class | [trendline](https://ej2.syncfusion.com/angular/documentation/api/chart/trendline) |
| TrendlineDirective | Trendline directive | [trendlineDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/trendlineDirective) |
| TrendlineTypes | Types enum (Linear, Exponential, Polynomial, Power, Logarithmic, MovingAverage) | [trendlineTypes](https://ej2.syncfusion.com/angular/documentation/api/chart/trendlineTypes) |

### Technical Indicators

| API | Description | Documentation Link |
|-----|-------------|----------|
| TechnicalIndicatorModel | Indicator configuration interface | [technicalIndicatorModel](https://ej2.syncfusion.com/angular/documentation/api/chart/technicalIndicatorModel) |
| TechnicalIndicator | Indicator class | [technicalIndicator](https://ej2.syncfusion.com/angular/documentation/api/chart/technicalIndicator) |
| IndicatorDirective | Indicator directive | [indicatorDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/indicatorDirective) |
| TechnicalIndicators | Indicator types enum (SMA, EMA, TMA, Momentum, ATR, RSI, Stochastic, BollingerBands, MACD, etc.) | [technicalIndicators](https://ej2.syncfusion.com/angular/documentation/api/chart/technicalIndicators) |
| MacdType | MACD type enum | [macdType](https://ej2.syncfusion.com/angular/documentation/api/chart/macdType) |

### Striplines

| API | Description | Documentation Link |
|-----|-------------|----------|
| StripLineSettingsModel | Stripline configuration interface | [stripLineSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/stripLineSettingsModel) |
| StripLineSettings | Stripline settings class | [stripLineSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/stripLineSettings) |
| StripLine | Stripline class | [stripLine](https://ej2.syncfusion.com/angular/documentation/api/chart/stripLine) |
| StripLineDirective | Stripline directive | [stripLineDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/stripLineDirective) |

## Interactive Feature APIs

### Zooming and Panning

| API | Description | Documentation Link |
|-----|-------------|----------|
| ZoomSettingsModel | Complete zoom configuration interface | [zoomSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettingsModel) |
| ZoomSettings | Zoom settings class | [zoomSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomSettings) |
| ZoomMode | Zoom mode enum (X, Y, XY) | [zoomMode](https://ej2.syncfusion.com/angular/documentation/api/chart/zoomMode) |
| ToolbarItems | Zoom toolbar items enum | [toolbarItems](https://ej2.syncfusion.com/angular/documentation/api/chart/toolbarItems) |
| ToolbarPosition | Toolbar position | [toolbarPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/toolbarPosition) |
| ScrollbarSettingsModel | Scrollbar configuration interface | [scrollbarSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/scrollbarSettingsModel) |
| ScrollbarSettings | Scrollbar settings class | [scrollbarSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/scrollbarSettings) |
| ScrollbarPosition | Scrollbar position enum | [scrollbarPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/scrollbarPosition) |
| ScrollbarSettingsRangeModel | Scrollbar range model | [scrollbarSettingsRangeModel](https://ej2.syncfusion.com/angular/documentation/api/chart/scrollbarSettingsRangeModel) |

### Crosshair

| API | Description | Documentation Link |
|-----|-------------|----------|
| CrosshairSettingsModel | Crosshair configuration interface | [crosshairSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshairSettingsModel) |
| CrosshairSettings | Crosshair settings class | [crosshairSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshairSettings) |
| Crosshair | Crosshair class | [crosshair](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshair) |
| CrosshairTooltipModel | Crosshair tooltip configuration | [crosshairTooltipModel](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshairTooltipModel) |
| CrosshairTooltip | Crosshair tooltip class | [crosshairTooltip](https://ej2.syncfusion.com/angular/documentation/api/chart/crosshairTooltip) |
| LineType | Line type enum | [lineType](https://ej2.syncfusion.com/angular/documentation/api/chart/lineType) |

### Selection and Highlighting

| API | Description | Documentation Link |
|-----|-------------|----------|
| SelectionMode | Selection mode enum (None, Point, Series, Cluster, DragXY, DragX, DragY, Lasso) | [selectionMode](https://ej2.syncfusion.com/angular/documentation/api/chart/selectionMode) |
| SelectionPattern | Selection pattern enum (None, Dots, DiagonalForward, Crosshatch, Pacman, etc.) | [selectionPattern](https://ej2.syncfusion.com/angular/documentation/api/chart/selectionPattern) |
| HighlightMode | Highlight mode enum (None, Point, Series, Cluster) | [highlightMode](https://ej2.syncfusion.com/angular/documentation/api/chart/highlightMode) |
| Highlight | Highlight settings | [highlight](https://ej2.syncfusion.com/angular/documentation/api/chart/highlight) |
| Selection | Selection settings | [selection](https://ej2.syncfusion.com/angular/documentation/api/chart/selection) |
| SelectedDataIndexDirective | Selected data index directive | [selectedDataIndexDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/selectedDataIndexDirective) |

### Data Editing

| API | Description | Documentation Link |
|-----|-------------|----------|
| DragSettingsModel | Drag configuration interface | [dragSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/dragSettingsModel) |
| DragSettings | Drag settings class | [dragSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/dragSettings) |

## Axis Configuration APIs

### Axis Properties

| API | Description | Documentation Link |
|-----|-------------|----------|
| AxisModel | Complete axis configuration (50+ properties) | [axisModel](https://ej2.syncfusion.com/angular/documentation/api/chart/axisModel) |
| Axis | Axis class | [axis](https://ej2.syncfusion.com/angular/documentation/api/chart/axis) |
| AxisDirective | Axis directive for multiple axes | [axisDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/axisDirective) |
| ValueType | Axis value type enum (Double, DateTime, DateTimeCategory, Category, Logarithmic) | [valueType](https://ej2.syncfusion.com/angular/documentation/api/chart/valueType) |
| IntervalType | DateTime interval enum (Years, Months, Days, Hours, Minutes, Seconds, Auto) | [intervalType](https://ej2.syncfusion.com/angular/documentation/api/chart/intervalType) |
| SkeletonType | DateTime skeleton types | [skeletonType](https://ej2.syncfusion.com/angular/documentation/api/chart/skeletonType) |
| ChartRangePadding | Axis padding modes (None, Normal, Additional, Round, Auto) | [chartRangePadding](https://ej2.syncfusion.com/angular/documentation/api/chart/chartRangePadding) |
| AxisPosition | Axis position enum (Inside, Outside) | [axisPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/axisPosition) |
| EdgeLabelPlacement | Edge label placement (None, Hide, Shift) | [edgeLabelPlacement](https://ej2.syncfusion.com/angular/documentation/api/chart/edgeLabelPlacement) |

### Axis Labels

| API | Description | Documentation Link |
|-----|-------------|----------|
| LabelIntersectAction | Label overlap handling (None, Hide, Trim, Wrap, MultipleRows, Rotate45, Rotate90) | [labelIntersectAction](https://ej2.syncfusion.com/angular/documentation/api/chart/labelIntersectAction) |
| LabelPlacement | Label placement relative to ticks (BetweenTicks, OnTicks) | [labelPlacement](https://ej2.syncfusion.com/angular/documentation/api/chart/labelPlacement) |
| LabelPosition | Label position enum | [labelPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/labelPosition) |
| LabelOverflow | Label overflow behavior | [labelOverflow](https://ej2.syncfusion.com/angular/documentation/api/chart/labelOverflow) |
| LabelBorderModel | Label border configuration | [labelBorderModel](https://ej2.syncfusion.com/angular/documentation/api/chart/labelBorderModel) |
| LabelBorder | Label border class | [labelBorder](https://ej2.syncfusion.com/angular/documentation/api/chart/labelBorder) |

### Gridlines and Tick Lines

| API | Description | Documentation Link |
|-----|-------------|----------|
| MajorGridLinesModel | Major gridline configuration | [majorGridLinesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/majorGridLinesModel) |
| MajorGridLines | Major gridlines class | [majorGridLines](https://ej2.syncfusion.com/angular/documentation/api/chart/majorGridLines) |
| MinorGridLinesModel | Minor gridline configuration | [minorGridLinesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/minorGridLinesModel) |
| MinorGridLines | Minor gridlines class | [minorGridLines](https://ej2.syncfusion.com/angular/documentation/api/chart/minorGridLines) |
| MajorTickLinesModel | Major tick line configuration | [majorTickLinesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/majorTickLinesModel) |
| MajorTickLines | Major tick lines class | [majorTickLines](https://ej2.syncfusion.com/angular/documentation/api/chart/majorTickLines) |
| MinorTickLinesModel | Minor tick line configuration | [minorTickLinesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/minorTickLinesModel) |
| MinorTickLines | Minor tick lines class | [minorTickLines](https://ej2.syncfusion.com/angular/documentation/api/chart/minorTickLines) |
| AxisLineModel | Axis line configuration | [axisLineModel](https://ej2.syncfusion.com/angular/documentation/api/chart/axisLineModel) |
| AxisLine | Axis line class | [axisLine](https://ej2.syncfusion.com/angular/documentation/api/chart/axisLine) |

### Multi-Level Labels

| API | Description | Documentation Link |
|-----|-------------|----------|
| MultiLevelLabelsModel | Multi-level label configuration | [multiLevelLabelsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/multiLevelLabelsModel) |
| MultiLevelLabels | Multi-level labels class | [multiLevelLabels](https://ej2.syncfusion.com/angular/documentation/api/chart/multiLevelLabels) |
| MultiLevelLabelDirective | Multi-level label directive | [multiLevelLabelDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/multiLevelLabelDirective) |
| MultiLevelCategoriesModel | Multi-level categories configuration | [multiLevelCategoriesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/multiLevelCategoriesModel) |
| MultiLevelCategories | Multi-level categories class | [multiLevelCategories](https://ej2.syncfusion.com/angular/documentation/api/chart/multiLevelCategories) |

## Series Configuration APIs

### Series Types

| API | Description | Documentation Link |
|-----|-------------|----------|
| ChartSeriesType | All series types enum (Line, Column, Bar, Area, Spline, Scatter, Bubble, Candle, Hilo, Histogram, BoxAndWhisker, Pareto, Polar, Radar, Waterfall, StackingLine, StackingColumn, StackingArea, and 10+ more) | [chartSeriesType](https://ej2.syncfusion.com/angular/documentation/api/chart/chartSeriesType) |
| ChartDrawType | Draw types for Polar/Radar (Line, Column, Area, Scatter, Spline, StackingArea, StackingColumn, RangeColumn) | [chartDrawType](https://ej2.syncfusion.com/angular/documentation/api/chart/chartDrawType) |

### Series Styling

| API | Description | Documentation Link |
|-----|-------------|----------|
| BorderModel | Border configuration | [borderModel](https://ej2.syncfusion.com/angular/documentation/api/chart/borderModel) |
| Border | Border class | [border](https://ej2.syncfusion.com/angular/documentation/api/chart/border) |
| BorderType | Border type enum | [borderType](https://ej2.syncfusion.com/angular/documentation/api/chart/borderType) |
| CornerRadiusModel | Corner radius configuration | [cornerRadiusModel](https://ej2.syncfusion.com/angular/documentation/api/chart/cornerRadiusModel) |
| CornerRadius | Corner radius class | [cornerRadius](https://ej2.syncfusion.com/angular/documentation/api/chart/cornerRadius) |
| ConnectorModel | Connector line configuration | [connectorModel](https://ej2.syncfusion.com/angular/documentation/api/chart/connectorModel) |
| Connector | Connector class | [connector](https://ej2.syncfusion.com/angular/documentation/api/chart/connector) |

### Financial Series

| API | Description | Documentation Link |
|-----|-------------|----------|
| FinancialDataFields | OHLC data field interface | [financialDataFields](https://ej2.syncfusion.com/angular/documentation/api/chart/financialDataFields) |

### Specialized Series

| API | Description | Documentation Link |
|-----|-------------|----------|
| SplineType | Spline interpolation types (Natural, Monotonic, Cardinal, Clamped) | [splineType](https://ej2.syncfusion.com/angular/documentation/api/chart/splineType) |
| BoxPlotMode | Box plot calculation modes (Normal, Exclusive, Inclusive) | [boxPlotMode](https://ej2.syncfusion.com/angular/documentation/api/chart/boxPlotMode) |
| ParetoOptionsModel | Pareto line configuration | [paretoOptionsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/paretoOptionsModel) |
| ParetoOptions | Pareto options class | [paretoOptions](https://ej2.syncfusion.com/angular/documentation/api/chart/paretoOptions) |
| StepPosition | Step line position (Left, Center, Right) | [stepPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/stepPosition) |

### Empty Points and Error Bars

| API | Description | Documentation Link |
|-----|-------------|----------|
| EmptyPointSettingsModel | Empty point configuration | [emptyPointSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/emptyPointSettingsModel) |
| EmptyPointSettings | Empty point settings class | [emptyPointSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/emptyPointSettings) |
| EmptyPointMode | Empty point modes (Gap, Zero, Average, Drop) | [emptyPointMode](https://ej2.syncfusion.com/angular/documentation/api/chart/emptyPointMode) |
| ErrorBarSettingsModel | Error bar configuration | [errorBarSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/errorBarSettingsModel) |
| ErrorBarSettings | Error bar settings class | [errorBarSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/errorBarSettings) |
| ErrorBarType | Error bar types (Fixed, Percentage, StandardDeviation, StandardError, Custom) | [errorBarType](https://ej2.syncfusion.com/angular/documentation/api/chart/errorBarType) |
| ErrorBarMode | Error bar modes (Vertical, Horizontal, Both) | [errorBarMode](https://ej2.syncfusion.com/angular/documentation/api/chart/errorBarMode) |
| ErrorBarDirection | Error bar direction (Both, Plus, Minus) | [errorBarDirection](https://ej2.syncfusion.com/angular/documentation/api/chart/errorBarDirection) |
| ErrorBarCapSettingsModel | Error bar cap configuration | [errorBarCapSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/errorBarCapSettingsModel) |
| ErrorBarCapSettings | Error bar cap settings class | [errorBarCapSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/errorBarCapSettings) |

### Color Mapping

| API | Description | Documentation Link |
|-----|-------------|----------|
| RangeColorSettingModel | Range color mapping configuration | [rangeColorSettingModel](https://ej2.syncfusion.com/angular/documentation/api/chart/rangeColorSettingModel) |
| RangeColorSetting | Range color setting class | [rangeColorSetting](https://ej2.syncfusion.com/angular/documentation/api/chart/rangeColorSetting) |
| RangeColorSettingDirective | Range color setting directive | [rangeColorSettingDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/rangeColorSettingDirective) |

## Chart Layout APIs

### Chart Area

| API | Description | Documentation Link |
|-----|-------------|----------|
| ChartAreaModel | Chart area configuration | [chartAreaModel](https://ej2.syncfusion.com/angular/documentation/api/chart/chartAreaModel) |
| ChartArea | Chart area class | [chartArea](https://ej2.syncfusion.com/angular/documentation/api/chart/chartArea) |
| MarginModel | Margin configuration | [marginModel](https://ej2.syncfusion.com/angular/documentation/api/chart/marginModel) |
| Margin | Margin class | [margin](https://ej2.syncfusion.com/angular/documentation/api/chart/margin) |
| ContainerPaddingModel | Container padding configuration | [containerPaddingModel](https://ej2.syncfusion.com/angular/documentation/api/chart/containerPaddingModel) |
| ContainerPadding | Container padding class | [containerPadding](https://ej2.syncfusion.com/angular/documentation/api/chart/containerPadding) |

### Rows and Columns

| API | Description | Documentation Link |
|-----|-------------|----------|
| RowModel | Row definition for multiple panes | [rowModel](https://ej2.syncfusion.com/angular/documentation/api/chart/rowModel) |
| Row | Row class | [row](https://ej2.syncfusion.com/angular/documentation/api/chart/row) |
| RowDirective | Row directive | [rowDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/rowDirective) |
| ColumnModel | Column definition for multiple panes | [columnModel](https://ej2.syncfusion.com/angular/documentation/api/chart/columnModel) |
| Column | Column class | [column](https://ej2.syncfusion.com/angular/documentation/api/chart/column) |
| ColumnDirective | Column directive | [columnDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/columnDirective) |

### Title and Styling

| API | Description | Documentation Link |
|-----|-------------|----------|
| TitleSettingsModel | Title configuration | [titleSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/titleSettingsModel) |
| TitleSettings | Title settings class | [titleSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/titleSettings) |
| TitleStyleSettingsModel | Title style configuration | [titleStyleSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/titleStyleSettingsModel) |
| TitleStyleSettings | Title style settings class | [titleStyleSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/titleStyleSettings) |
| TitlePosition | Title position enum | [titlePosition](https://ej2.syncfusion.com/angular/documentation/api/chart/titlePosition) |
| TitleBorderModel | Title border configuration | [titleBorderModel](https://ej2.syncfusion.com/angular/documentation/api/chart/titleBorderModel) |
| TitleBorder | Title border class | [titleBorder](https://ej2.syncfusion.com/angular/documentation/api/chart/titleBorder) |

### Theme and Appearance

| API | Description | Documentation Link |
|-----|-------------|----------|
| ChartTheme | Theme enum (Material, Bootstrap, Fabric, Bootstrap4, Tailwind, TailwindDark, Bootstrap5, Bootstrap5Dark, Fluent, FluentDark, Material3, Material3Dark, and more) | [chartTheme](https://ej2.syncfusion.com/angular/documentation/api/chart/chartTheme) |
| FontModel | Font configuration | [fontModel](https://ej2.syncfusion.com/angular/documentation/api/chart/fontModel) |
| Font | Font class | [font](https://ej2.syncfusion.com/angular/documentation/api/chart/font) |
| LocationModel | Location/position configuration | [locationModel](https://ej2.syncfusion.com/angular/documentation/api/chart/locationModel) |
| Documentation Link | Location class | [location](https://ej2.syncfusion.com/angular/documentation/api/chart/location) |
| OffsetModel | Offset configuration | [offsetModel](https://ej2.syncfusion.com/angular/documentation/api/chart/offsetModel) |
| Offset | Offset class | [offset](https://ej2.syncfusion.com/angular/documentation/api/chart/offset) |

## Animation and Performance APIs

| API | Description | Documentation Link |
|-----|-------------|----------|
| AnimationModel | Animation configuration | [animationModel](https://ej2.syncfusion.com/angular/documentation/api/chart/animationModel) |
| Animation | Animation class | [animation](https://ej2.syncfusion.com/angular/documentation/api/chart/animation) |

## Export and Print APIs

| API | Description | Documentation Link |
|-----|-------------|----------|
| ExportType | Export format enum (PNG, JPEG, SVG, PDF, XLSX, CSV) | [exportType](https://ej2.syncfusion.com/angular/documentation/api/chart/exportType) |
| Export | Export utility class | [export](https://ej2.syncfusion.com/angular/documentation/api/chart/export) |
| PrintUtils | Print utility functions | [printUtils](https://ej2.syncfusion.com/angular/documentation/api/chart/printUtils) |
| Units | Size units enum | [units](https://ej2.syncfusion.com/angular/documentation/api/chart/units) |

## Event Interfaces

### Lifecycle Events

| Event Interface | Description | Documentation Link |
|-----------------|-------------|----------|
| ILoadedEventArgs | Chart load events (load, loaded) | [iLoadedEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iLoadedEventArgs) |
| IChartEventArgs | Base chart event arguments | [iChartEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iChartEventArgs) |
| IAnimationCompleteEventArgs | Animation complete event | [iAnimationCompleteEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iAnimationCompleteEventArgs) |

### Rendering Events

| Event Interface | Description | Documentation Link |
|-----------------|-------------|----------|
| IPointRenderEventArgs | Point rendering event | [iPointRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPointRenderEventArgs) |
| ISeriesRenderEventArgs | Series rendering event | [iSeriesRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iSeriesRenderEventArgs) |
| IAxisLabelRenderEventArgs | Axis label rendering event | [iAxisLabelRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iAxisLabelRenderEventArgs) |
| IAxisMultiLabelRenderEventArgs | Multi-level axis label rendering | [iAxisMultiLabelRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iAxisMultiLabelRenderEventArgs) |
| ILegendRenderEventArgs | Legend rendering event | [iLegendRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iLegendRenderEventArgs) |
| ITextRenderEventArgs | Text rendering event (data labels) | [iTextRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iTextRenderEventArgs) |
| IAnnotationRenderEventArgs | Annotation rendering event | [iAnnotationRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iAnnotationRenderEventArgs) |

### Interaction Events

| Event Interface | Description | Documentation Link |
|-----------------|-------------|----------|
| IMouseEventArgs | Mouse events (click, move, down, up, leave) | [iMouseEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iMouseEventArgs) |
| IPointEventArgs | Point events (click, doubleClick, move) | [iPointEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPointEventArgs) |
| ITooltipRenderEventArgs | Tooltip rendering event | [iTooltipRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iTooltipRenderEventArgs) |
| ISharedTooltipRenderEventArgs | Shared tooltip rendering | [iSharedTooltipRenderEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iSharedTooltipRenderEventArgs) |
| ILegendClickEventArgs | Legend click event | [iLegendClickEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iLegendClickEventArgs) |
| IAxisLabelClickEventArgs | Axis label click event | [iAxisLabelClickEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iAxisLabelClickEventArgs) |
| IMultiLevelLabelClickEventArgs | Multi-level label click | [iMultiLevelLabelClickEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iMultiLevelLabelClickEventArgs) |

### Feature Events

| Event Interface | Description | Documentation Link |
|-----------------|-------------|----------|
| IZoomingEventArgs | During zoom operation | [iZoomingEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iZoomingEventArgs) |
| IZoomCompleteEventArgs | After zoom completes | [iZoomCompleteEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iZoomCompleteEventArgs) |
| IScrollEventArgs | Scrollbar events (start, end, changed) | [iScrollEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iScrollEventArgs) |
| ISelectionCompleteEventArgs | After selection completes | [iSelectionCompleteEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iSelectionCompleteEventArgs) |
| IDragCompleteEventArgs | Drag operation complete | [iDragCompleteEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iDragCompleteEventArgs) |
| IDataEditingEventArgs | Data editing event | [iDataEditingEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iDataEditingEventArgs) |

### Export and Print Events

| Event Interface | Description | Documentation Link |
|-----------------|-------------|----------|
| IExportEventArgs | Export events (beforeExport, afterExport) | [iExportEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iExportEventArgs) |
| IPrintEventArgs | Print events (beforePrint) | [iPrintEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPrintEventArgs) |
| IPDFArgs | PDF export arguments | [iPDFArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iPDFArgs) |

### Resize Events

| Event Interface | Description | Documentation Link |
|-----------------|-------------|----------|
| IResizeEventArgs | Resize events (resized) | [iResizeEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iResizeEventArgs) |
| IBeforeResizeEventArgs | Before resize event | [iBeforeResizeEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iBeforeResizeEventArgs) |

### Data Events

| Event Interface | Description | Documentation Link |
|-----------------|-------------|----------|
| IAxisRangeCalculatedEventArgs | After axis range calculation | [iAxisRangeCalculatedEventArgs](https://ej2.syncfusion.com/angular/documentation/api/chart/iAxisRangeCalculatedEventArgs) |

## Utility and Helper APIs

### Points and Data

| API | Description | Documentation Link |
|-----|-------------|----------|
| Points | Points data structure | [points](https://ej2.syncfusion.com/angular/documentation/api/chart/points) |
| Indexes | Index collection | [indexes](https://ej2.syncfusion.com/angular/documentation/api/chart/indexes) |
| IndexesModel | Index model | [indexesModel](https://ej2.syncfusion.com/angular/documentation/api/chart/indexesModel) |

### Stock Chart Specific

| API | Description | Documentation Link |
|-----|-------------|----------|
| StockTooltipSettingsModel | Stock tooltip configuration | [stockTooltipSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/stockTooltipSettingsModel) |
| StockTooltipSettings | Stock tooltip settings class | [stockTooltipSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/stockTooltipSettings) |
| PeriodSelectorSettingsModel | Period selector configuration | [periodSelectorSettingsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/periodSelectorSettingsModel) |
| PeriodSelectorSettings | Period selector settings class | [periodSelectorSettings](https://ej2.syncfusion.com/angular/documentation/api/chart/periodSelectorSettings) |
| PeriodSelectorPosition | Period selector position enum | [periodSelectorPosition](https://ej2.syncfusion.com/angular/documentation/api/chart/periodSelectorPosition) |
| PeriodsModel | Periods configuration | [periodsModel](https://ej2.syncfusion.com/angular/documentation/api/chart/periodsModel) |
| Periods | Periods class | [periods](https://ej2.syncfusion.com/angular/documentation/api/chart/periods) |

### Accessibility

| API | Description | Documentation Link |
|-----|-------------|----------|
| AccessibilityModel | Accessibility configuration | [accessibilityModel](https://ej2.syncfusion.com/angular/documentation/api/chart/accessibilityModel) |
| Accessibility | Accessibility class | [accessibility](https://ej2.syncfusion.com/angular/documentation/api/chart/accessibility) |
| SeriesAccessibilityModel | Series accessibility configuration | [seriesAccessibilityModel](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesAccessibilityModel) |
| SeriesAccessibility | Series accessibility class | [seriesAccessibility](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesAccessibility) |

### Miscellaneous

| API | Description | Documentation Link |
|-----|-------------|----------|
| CenterLabelModel | Center label configuration (for pie/donut) | [centerLabelModel](https://ej2.syncfusion.com/angular/documentation/api/chart/centerLabelModel) |
| CenterLabel | Center label class | [centerLabel](https://ej2.syncfusion.com/angular/documentation/api/chart/centerLabel) |
| SeriesBase | Base series class | [seriesBase](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesBase) |
| SeriesBaseModel | Base series model | [seriesBaseModel](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesBaseModel) |
| CategoryDirective | Category directive | [categoryDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/categoryDirective) |
| SegmentDirective | Segment directive for multi-colored series | [segmentDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/segment) |
| ChartSegmentModel | Chart segment model | [chartSegmentModel](https://ej2.syncfusion.com/angular/documentation/api/chart/chartSegmentModel) |
| ChartSegment | Chart segment class | [chartSegment](https://ej2.syncfusion.com/angular/documentation/api/chart/chartSegment) |
| RegionModel | Region configuration | [regions](https://ej2.syncfusion.com/angular/documentation/api/chart/regions) |
| FlagType | Flag type enum | [flagType](https://ej2.syncfusion.com/angular/documentation/api/chart/flagType) |
| SizeType | Size type enum | [sizeType](https://ej2.syncfusion.com/angular/documentation/api/chart/sizeType) |
| ShapeType | Shape type enum | [shapeType](https://ej2.syncfusion.com/angular/documentation/api/chart/shapeType) |
| zIndex | Z-index configuration | [zIndex](https://ej2.syncfusion.com/angular/documentation/api/chart/zIndex) |
| StaticFunctions | Static utility functions | [staticFunctions](https://ej2.syncfusion.com/angular/documentation/api/chart/staticFunctions) |
| Overview | API overview | [overview](https://ej2.syncfusion.com/angular/documentation/api/chart/overview) |
| Summary | API summary | [summary](https://ej2.syncfusion.com/angular/documentation/api/chart/summary) |

## How to Navigate the API Documentation

### Quick Start Path

1. **Start with Core APIs:**
   - Read [ChartModel](https://ej2.syncfusion.com/angular/documentation/api/chart/chartModel) - Main chart configuration with all 40+ properties and events
   - Review [AxisModel](https://ej2.syncfusion.com/angular/documentation/api/chart/axisModel) - Axis configuration with 50+ properties
   - Explore [SeriesDirective](https://ej2.syncfusion.com/angular/documentation/api/chart/seriesDirective) - Series configuration with 60+ properties

2. **Feature-Specific APIs:**
   - Look up specific feature models as needed (Legend, Tooltip, Zoom, Marker, DataLabel)
   - Reference event interfaces when implementing event handlers
   - Check enumerations for valid values (ChartSeriesType, ValueType, ChartTheme, etc.)

3. **Advanced Usage:**
   - Review specialized APIs for financial charts, technical indicators, annotations
   - Check utility models for styling, animation, borders, fonts
   - Explore export/print utilities for sharing capabilities

### Finding Specific APIs

**By Feature:**
- Legend: LegendSettingsModel, LegendPosition, LegendShape
- Tooltip: TooltipSettingsModel, TooltipPosition
- Zoom: ZoomSettingsModel, ZoomMode, ToolbarItems
- Selection: SelectionMode, SelectionPattern, HighlightMode
- Series Types: ChartSeriesType, ChartDrawType
- Axes: AxisModel, ValueType, IntervalType, LabelIntersectAction

**By Task:**
- Styling: BorderModel, FontModel, MarginModel, CornerRadiusModel
- Data Binding: FinancialDataFields, EmptyPointSettingsModel
- Interaction: IMouseEventArgs, IPointEventArgs, ITooltipRenderEventArgs
- Export: ExportType, IExportEventArgs, IPrintEventArgs
- Layout: ChartAreaModel, RowModel, ColumnModel, TitleSettingsModel

### API Documentation Structure

Each API file contains:
- **Property Descriptions:** Type, default value, description
- **Enumeration Values:** All available options
- **Usage Examples:** Common patterns
- **Related APIs:** Cross-references to related interfaces/classes

### Complete API File List

**Location:** All 200+ API files are in the `chart/` directory
**Organization:** Alphabetical by API name with Model suffix for interfaces

**Categories:**
- **Configuration Models:** xxxModel.md (100+ files)
- **Implementation Classes:** xxx.md without Model suffix (30+ files)
- **Enumerations:** Enum types (50+ files)
- **Event Interfaces:** Ixxx.md files (40+ files)
- **Directives:** xxxDirective.md (15+ files)
- **Utilities:** Helper and utility classes (20+ files)

## Integration with Reference Guides

All 10 reference guides link to relevant API documentation:

1. **getting-started.md** → ChartModel, AxisModel, SeriesDirective, TooltipSettingsModel, LegendSettingsModel
2. **series-types.md** → ChartSeriesType, SeriesDirective, Series properties (columnWidth, bearFillColor, binInterval, etc.)
3. **axes-and-layout.md** → AxisModel, ValueType, IntervalType, LabelIntersectAction, ChartRangePadding
4. **chart-elements.md** → Legend, Marker, DataLabel, Tooltip, Annotation, Trendline, TechnicalIndicator, StripLine APIs
5. **interactive-features.md** → ZoomSettingsModel, TooltipSettingsModel, CrosshairSettingsModel, SelectionMode
6. **data-binding.md** → Series.dataSource, EmptyPointSettingsModel, FinancialDataFields
7. **customization.md** → ChartTheme, BorderModel, FontModel, AnimationModel
8. **accessibility.md** → AccessibilityModel, SeriesAccessibilityModel
9. **advanced-features.md** → All 40+ event interfaces, ExportType, IExportEventArgs
10. **common-patterns.md** → Practical combinations of multiple APIs

---

**For the most up-to-date and comprehensive API documentation, always refer to the individual API files in the chart/ directory.**
