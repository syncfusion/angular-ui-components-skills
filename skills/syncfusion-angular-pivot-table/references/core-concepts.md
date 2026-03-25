# Core Concepts & Data Binding

## Table of Contents
- [Data Binding Types](#data-binding-types)
- [JSON Data Binding](#json-data-binding)
- [CSV Data Binding](#csv-data-binding)
- [Remote Data Binding](#remote-data-binding)
- [OLAP Data Binding](#olap-data-binding)
- [Choosing the Right Data Source](#choosing-the-right-data-source)

## Data Binding Types

The Pivot Table supports four primary data binding approaches, each optimized for different scenarios and data volumes:

### 1. **JSON (Local & Remote)**
- Best for small to medium datasets (up to 100K rows)
- Works with in-memory arrays or remote JSON endpoints
- Client-side processing using the built-in Pivot Engine
- Fastest setup and deployment

### 2. **CSV (Local & Remote)**
- Compact format (approximately half the size of JSON)
- Good for relational data and spreadsheet-style datasets
- Requires conversion to string arrays for local data
- Reduced bandwidth usage for remote sources

### 3. **OLAP (Cube-Based)**
- Designed for pre-aggregated, multi-dimensional data
- Connects to SQL Server Analysis Services (SSAS), Mondrian, or other OLAP servers
- Ideal for enterprise BI scenarios
- Supports complex hierarchies and calculated members

### 4. **Server-Side Pivot Engine**
- Processes data on the ASP.NET Core server instead of the client
- Essential for large datasets (100K+ rows)
- Reduces network traffic by sending only viewport data
- Best performance for complex operations

---

## JSON Data Binding

JSON is the default data type for the Pivot Table. Binding JSON data provides flexible options for both local in-memory data and remote sources.

### Binding Local JSON Array

The simplest approach is to assign a local variable containing JSON data to the [`dataSource`](https://ej2.syncfusion.com/angular/documentation/api/pivotview/dataSourceSettings/#datasource) property:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
    selector: 'app-container',
    template: `<ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;

    ngOnInit(): void {
        this.dataSourceSettings = {
            dataSource: [
                { 'Sold': 31, 'Amount': 52824, 'Country': 'France', 'Products': 'Mountain Bikes', 'Year': 'FY 2015', 'Quarter': 'Q1' }
            ],
            rows: [{ name: 'Country' }],
            columns: [{ name: 'Year' }],
            values: [{ name: 'Amount', type: 'Sum' }]
        };
    }
}
```

### Using DataManager with JsonAdaptor

For more flexible data management, use the [`DataManager`](https://ej2.syncfusion.com/angular/documentation/api/pivotview/dataSourceSettings/#datasource) with `JsonAdaptor`:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataManager, JsonAdaptor } from '@syncfusion/ej2-data';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
    selector: 'app-container',
    template: `<ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;

    ngOnInit(): void {
        let data = [
            { 'Month': 'Jan', 'Company': 'Jet Airways', 'Profit': 21, 'Loss': -1 }
        ];

        this.dataSourceSettings = {
            dataSource: new DataManager({
                data: data,
                adaptor: new JsonAdaptor
            }),
            rows: [{ name: 'Month' }],
            columns: [{ name: 'Company' }],
            values: [
                { name: 'Profit', type: 'Sum' },
                { name: 'Loss', type: 'Sum' }
            ]
        };
    }
}
```

### Loading JSON from File Upload

To load JSON data from a local *.json file using the file uploader:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import { Uploader } from '@syncfusion/ej2-inputs';

@Component({
    selector: 'app-container',
    template: `
        <input type="file" id="fileupload">
        <ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>
    `
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;
    public uploadObj: Uploader;

    ngOnInit(): void {
        // Initialize the file uploader
        this.uploadObj = new Uploader({});
        this.uploadObj.appendTo('#fileupload');

        let input = document.querySelector('input[type="file"]');
        // Listen for file upload
        input.addEventListener('change', (e: Event) => {
            let reader = new FileReader();
            reader.onload = () => {
                // Parse the JSON string and bind to Pivot Table
                let result = JSON.parse(reader.result as string);
                this.dataSourceSettings = {
                    dataSource: result,
                    rows: [{ name: 'Country' }],
                    columns: [{ name: 'Product' }],
                    values: [{ name: 'Sales', type: 'Sum' }]
                };
            };
            reader.readAsText((input as any).files[0]);
        });
    }
}
```

### Binding Remote JSON Data

To connect to a remote JSON endpoint, set the [`url`](https://ej2.syncfusion.com/angular/documentation/api/pivotview/dataSourceSettings/#url) property:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
    selector: 'app-container',
    template: `<ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;

    ngOnInit(): void {
        this.dataSourceSettings = {
            url: 'https://api.example.com/pivot-data.json',
            rows: [{ name: 'Country' }],
            columns: [{ name: 'Product' }],
            values: [{ name: 'Sales', type: 'Sum' }]
        };
    }
}
```

---

## CSV Data Binding

CSV (Comma-Separated Values) format provides a compact alternative to JSON, using approximately half the bandwidth. Set the [`type`](https://ej2.syncfusion.com/angular/documentation/api/pivotview/dataSourceSettings/#type) property to `CSV`.

### Binding Local CSV Data

To bind local CSV data, convert it to a string array:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
    selector: 'app-container',
    template: `<ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;

    ngOnInit(): void {
        // CSV data as string array
        let csvData = [
            ['Country', 'Product', 'Sales', 'Year'],
            ['USA', 'Laptop', '5000', '2023']
        ];

        this.dataSourceSettings = {
            dataSource: csvData,
            type: 'CSV',
            rows: [{ name: 'Country' }],
            columns: [{ name: 'Product' }],
            values: [{ name: 'Sales', type: 'Sum' }]
        };
    }
}
```

### Loading CSV from File Upload

To load CSV data from a local *.csv file:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import { Uploader } from '@syncfusion/ej2-inputs';

@Component({
    selector: 'app-container',
    template: `
        <input type="file" id="fileupload">
        <ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>
    `
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;

    ngOnInit(): void {
        const input = document.querySelector('input[type="file"]');
        input.addEventListener('change', (e: Event) => {
            let reader = new FileReader();
            reader.onload = () => {
                // Convert CSV string to string array
                let csvArray = (reader.result as string).split('\n').map(line => line.split(','));
                this.dataSourceSettings = {
                    dataSource: csvArray,
                    type: 'CSV',
                    rows: [{ name: 'Country' }],
                    columns: [{ name: 'Product' }],
                    values: [{ name: 'Sales', type: 'Sum' }]
                };
            };
            reader.readAsText((input as any).files[0]);
        });
    }
}
```

### Binding Remote CSV Data

To connect to a remote CSV file or endpoint:

```typescript
this.dataSourceSettings = {
    url: 'https://your-server.com/data.csv',
    type: 'CSV',
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales', type: 'Sum' }]
};
```

---

## Remote Data Binding

Remote data binding connects to web services and external data sources through various adaptor types.

### OData Service Binding

Connect to OData (Open Data Protocol) services using the default [`ODataAdaptor`](https://ej2.syncfusion.com/angular/documentation/data/adaptors#odata-adaptor):

```typescript
import { Component, OnInit } from '@angular/core';
import { DataManager, ODataAdaptor } from '@syncfusion/ej2-data';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
    selector: 'app-container',
    template: `<ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;

    ngOnInit(): void {
        this.dataSourceSettings = {
            dataSource: new DataManager({
                url: 'https://js.syncfusion.com/demos/ejServices/Wcf/Northwind.svc/Orders',
                adaptor: new ODataAdaptor()
            }),
            rows: [{ name: 'CustomerID' }],
            columns: [{ name: 'OrderDate' }],
            values: [{ name: 'Freight', type: 'Sum' }]
        };
    }
}
```

### OData V4 Service Binding

For OData V4 services, use the [`ODataV4Adaptor`](https://ej2.syncfusion.com/angular/documentation/data/adaptors#odatav4-adaptor):

```typescript
import { DataManager, ODataV4Adaptor } from '@syncfusion/ej2-data';

this.dataSourceSettings = {
    dataSource: new DataManager({
        url: 'https://services.odata.org/v4/northwind/northwind.svc/Orders',
        adaptor: new ODataV4Adaptor()
    }),
    rows: [{ name: 'CustomerID' }],
    columns: [{ name: 'OrderDate' }],
    values: [{ name: 'Freight', type: 'Sum' }]
};
```

### Web API Binding

Connect to RESTful Web APIs using the [`WebApiAdaptor`](https://ej2.syncfusion.com/angular/documentation/data/adaptors#web-api-adaptor):

```typescript
import { DataManager, WebApiAdaptor } from '@syncfusion/ej2-data';

this.dataSourceSettings = {
    dataSource: new DataManager({
        url: 'https://api.example.com/pivot-data',
        adaptor: new WebApiAdaptor()
    }),
    rows: [{ name: 'Region' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales', type: 'Sum' }]
};
```

### Dynamic Data Loading with HttpClient

For more control over data fetching, use Angular's HttpClient:

```typescript
import { HttpClient } from '@angular/common/http';
import { Component, OnInit } from '@angular/core';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
    selector: 'app-container',
    template: `<ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel = {};

    constructor(private http: HttpClient) { }

    ngOnInit(): void {
        this.http.get('https://api.example.com/sales-data').subscribe((data: any) => {
            this.dataSourceSettings = {
                dataSource: data.records,
                rows: [{ name: 'Region' }],
                columns: [{ name: 'Product' }],
                values: [{ name: 'Sales', type: 'Sum' }]
            };
        });
    }
}
```

## OLAP Data Binding

OLAP (Online Analytical Processing) connects to cube-based data sources like SQL Server Analysis Services (SSAS) or Mondrian. OLAP data is pre-aggregated and organized in multi-dimensional hierarchies.

### OLAP Data Source Configuration

Configure the Pivot Table to connect to an OLAP cube:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
    selector: 'app-container',
    template: `<ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;

    ngOnInit(): void {
        this.dataSourceSettings = {
            catalog: 'Adventure Works DW 2008 SE',
            cube: 'Adventure Works',
            providerType: 'SSAS',
            enableSorting: true,
            url: 'https://bi.syncfusion.com/olap/msmdpump.dll',
            localeIdentifier: 1033
        };
    }
}
```

### Adding OLAP Cube Elements

Define rows, columns, values, and filters using OLAP hierarchies and measures:

```typescript
this.dataSourceSettings = {
    catalog: 'Adventure Works DW 2008 SE',
    cube: 'Adventure Works',
    providerType: 'SSAS',
    url: 'https://bi.syncfusion.com/olap/msmdpump.dll',
    rows: [
        { name: '[Customer].[Customer Geography].[Country]', caption: 'Country' }
    ],
    columns: [
        { name: '[Product].[Product Categories].[Category]', caption: 'Product Category' }
    ],
    values: [
        { name: '[Measures].[Customer Count]', caption: 'Customer Count' },
        { name: '[Measures].[Internet Sales Amount]', caption: 'Internet Sales Amount' }
    ],
    filters: []
};
```

### When to Use OLAP

**Best for:**
- Multi-dimensional analysis
- Pre-aggregated enterprise data
- Complex hierarchies (Year → Quarter → Month)
- Large data volumes with fast query performance
- Organizations using SSAS or other OLAP servers

**Not ideal for:**
- Small datasets (overhead of cube infrastructure)
- Real-time transaction data
- Simple relational data analysis

## Server-Side Pivot Engine

For very large datasets (100K+ rows), the server-side Pivot Engine processes data on an ASP.NET Core server, reducing client-side load and improving performance.

### Prerequisites

1. Download and install the [Server-side Pivot Engine](https://github.com/SyncfusionExamples/server-side-pivot-engine-for-pivot-table) from GitHub
2. Ensure the application includes:
   - **PivotController.cs**: Handles client-server communication
   - **DataSource.cs**: Defines data source models
   - **Syncfusion.Pivot.Engine** NuGet package

### Connecting to Server-Side Engine

Configure the Pivot Table to use server-side mode:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

@Component({
    selector: 'app-container',
    template: `<ejs-pivotview [dataSourceSettings]=dataSourceSettings></ejs-pivotview>`
})
export class AppComponent implements OnInit {
    public dataSourceSettings: DataSourceSettingsModel;

    ngOnInit(): void {
        this.dataSourceSettings = {
            url: 'https://localhost:44350/api/pivot/post',
            mode: 'Server',
            rows: [{ name: 'ProductID', caption: 'Product ID' }],
            columns: [{ name: 'Year', caption: 'Production Year' }],
            values: [
                { name: 'Sold', caption: 'Units Sold' },
                { name: 'Price', caption: 'Sold Amount' }
            ],
            formatSettings: [
                { name: 'Price', format: 'C' }
            ]
        };
    }
}
```

### Server-Side Data Configuration

Define your data source in the server-side application's **DataSource.cs**:

```csharp
public class PivotViewData
{
    public string ProductID { get; set; }
    public string Country { get; set; }
    public string Product { get; set; }
    public double Sold { get; set; }
    public double Price { get; set; }
    public string Year { get; set; }

    public List<PivotViewData> GetData()
    {
        // Return data from database, CSV, JSON, or collection
    }
}
```

In **PivotController.cs**:

```csharp
private PivotEngine<PivotViewData> pivotEngine = new PivotEngine<PivotViewData>();

[HttpPost]
public async Task<object> GetData(FetchData param)
{
    return await _cache.GetOrCreateAsync("dataSource" + param.Hash, async (cacheEntry) =>
    {
        cacheEntry.SetSize(1);
        cacheEntry.AbsoluteExpiration = DateTimeOffset.UtcNow.AddMinutes(60);
        return new DataSource.PivotViewData().GetData();
    });
}
```

### Supported Server-Side Data Sources

- **Collection**: List<T> or IEnumerable
- **JSON**: From *.json files or API endpoints
- **CSV**: From *.csv files or web services
- **DataTable**: From SQL Server or other databases
- **Dynamic**: From any custom data source

---

## Choosing the Right Data Source

| Scenario | Recommended | Reason |
|----------|-------------|--------|
| **Small dataset (<50K rows)** | JSON or CSV | Fast, simple setup, client-side processing |
| **Medium dataset (50K-100K)** | JSON + DataManager | Flexible, optimized data loading |
| **Large dataset (>100K)** | Server-Side Engine | Reduces client load, optimal performance |
| **Multi-dimensional analysis** | OLAP | Pre-aggregated, hierarchical data |
| **Real-time transactions** | JSON remote | Direct database connection via API |
| **File uploads** | JSON/CSV file upload | Flexible user data import |
| **Enterprise BI** | OLAP or Server-Side | Integrated with enterprise infrastructure |
| **Bandwidth-limited** | CSV | Compact format, ~50% size of JSON |

### Performance Considerations

**Client-Side (JSON/CSV):**
- ✓ Instant local filtering/sorting
- ✓ Works offline
- ✗ Limited to ~100K rows
- ✗ Full dataset in memory

**Server-Side Engine:**
- ✓ Handles millions of rows
- ✓ Server performs heavy lifting
- ✓ Only viewport data sent to client
- ✗ Requires ASP.NET Core backend
- ✗ Network latency for operations

**OLAP:**
- ✓ Lightning-fast pre-aggregated queries
- ✓ Complex hierarchies
- ✗ Setup overhead
- ✗ Not for real-time data
