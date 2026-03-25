# File System Provider Reference in Syncfusion Angular File Manager

## Overview

The FileManager component supports multiple file system providers, allowing you to connect to various backend storage systems. Each provider offers specialized functionality for managing files and folders in different environments.

## Table of Contents
- [Available File System Providers](#available-file-system-providers)
- [Physical File System Provider](#physical-file-system-provider)
- [Azure Cloud File System Provider](#azure-cloud-file-system-provider)
- [Amazon S3 Cloud File Provider](#amazon-s3-cloud-file-provider)
- [SharePoint File Provider](#sharepoint-file-provider)
- [File Transfer Protocol Provider](#file-transfer-protocol-provider)
- [SQL Database File System Provider](#sql-database-file-system-provider)
- [Node.js File System Provider](#nodejs-file-system-provider)
- [Google Drive File System Provider](#google-drive-file-system-provider)
- [Firebase Realtime Database Provider](#firebase-realtime-database-provider)
- [Comparison Table](#comparison-table)

---

## Available File System Providers

| Provider | Type | Use Case | Complexity |
|----------|------|----------|-----------|
| Physical File System | On-Premise | Local server files | Low |
| Azure Blob Storage | Cloud | Azure cloud storage | Medium |
| Amazon S3 | Cloud | AWS object storage | Medium |
| SharePoint | Enterprise | Microsoft SharePoint | High |
| FTP | Network | FTP server files | Medium |
| SQL Database | Database | Database-backed storage | Medium |
| Node.js | Server | Node.js runtime | Medium |
| Google Drive | Cloud | Google Drive integration | High |
| Firebase | Cloud | Firebase cloud database | Medium |

---

## Physical File System Provider

### Overview

The Physical file system provider allows management of files and folders on a physical server or local storage. This is the most straightforward provider for on-premise solutions.

### Setup

#### Step 1: Clone the Repository

```bash
git clone https://github.com/SyncfusionExamples/ej2-aspcore-file-provider ej2-aspcore-file-provider
cd ej2-aspcore-file-provider
```

#### Step 2: Open in Visual Studio

1. Open the project in Visual Studio
2. Restore NuGet packages
3. Set the root directory in the File Manager controller

#### Step 3: Configure Root Directory

```csharp
// In FileManagerController.cs
public class FileManagerController : ControllerBase
{
    private string basePath = @"C:\Files"; // Set your root directory here
    
    [HttpPost("FileOperations")]
    public IActionResult FileOperations(string action, string path)
    {
        // Operations on physical file system
        // using basePath as root
        return Ok();
    }
}
```

#### Step 4: Build and Run

```bash
# Port will be displayed in console
# Default: http://localhost:5000
```

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:5000/api/FileManager/FileOperations',
    downloadUrl: 'http://localhost:5000/api/FileManager/Download',
    uploadUrl: 'http://localhost:5000/api/FileManager/Upload',
    getImageUrl: 'http://localhost:5000/api/FileManager/GetImage'
  };
}
```

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download | ✅ Search

### Repository

**GitHub:** [ej2-aspcore-file-provider](https://github.com/SyncfusionExamples/ej2-aspcore-file-provider)

---

## Azure Cloud File System Provider

### Overview

The Azure file system provider enables management of blobs (files) stored in Azure Blob Storage containers. Perfect for cloud-based deployments.

### Prerequisites

- Azure Storage Account
- Storage Account Name
- Storage Account Key
- Blob Container Name

### Setup

#### Step 1: Clone Repository

```bash
git clone https://github.com/SyncfusionExamples/azure-aspcore-file-provider azure-aspcore-file-provider
cd azure-aspcore-file-provider
```

#### Step 2: Register Azure Storage

```csharp
// In AzureProviderController.cs
public void RegisterAzure(string accountName, string accountKey, string blobName)
{
    // Register Azure Blob Storage
    // accountName: Your storage account name
    // accountKey: Your storage account key
    // blobName: Your container name
}
```

#### Step 3: Set Blob Container

```csharp
public void SetBlobContainer(string blobPath, string filePath)
{
    // blobPath: Container URL (e.g., https://account.blob.core.windows.net/container)
    // filePath: Root folder path within container
}
```

#### Step 4: Configure appsettings.json

```json
{
  "AzureSettings": {
    "AccountName": "your-account-name",
    "AccountKey": "your-account-key",
    "BlobContainer": "your-container-name",
    "BlobPath": "https://your-account.blob.core.windows.net/your-container",
    "FilePath": "/files"
  }
}
```

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:5000/api/AzureProvider/AzureFileOperations',
    downloadUrl: 'http://localhost:5000/api/AzureProvider/AzureDownload',
    uploadUrl: 'http://localhost:5000/api/AzureProvider/AzureUpload',
    getImageUrl: 'http://localhost:5000/api/AzureProvider/AzureGetImage'
  };
}
```

### Installation via NuGet

```bash
dotnet add package Syncfusion.EJ2.FileManager.AzureFileProvider.AspNet.Core
```

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download | ✅ Search

### Repository

**GitHub:** [azure-aspcore-file-provider](https://github.com/SyncfusionExamples/azure-aspcore-file-provider)
**NuGet:** [Syncfusion.EJ2.FileManager.AzureFileProvider.AspNet.Core](https://www.nuget.org/packages/Syncfusion.EJ2.FileManager.AzureFileProvider.AspNet.Core)

---

## Amazon S3 Cloud File Provider

### Overview

The Amazon S3 file provider allows management of objects stored in AWS S3 buckets. Ideal for scalable cloud storage solutions.

### Prerequisites

- AWS S3 Bucket
- AWS Access Key ID
- AWS Secret Access Key
- AWS Region

### Setup

#### Step 1: Clone Repository

```bash
git clone https://github.com/SyncfusionExamples/amazon-s3-aspcore-file-provider amazon-s3-aspcore-file-provider
cd amazon-s3-aspcore-file-provider
```

#### Step 2: Register AWS Credentials

```csharp
// In AmazonS3ProviderController.cs
public void RegisterAmazonS3(
    string bucketName, 
    string awsAccessKeyId, 
    string awsSecretAccessKey, 
    string bucketRegion)
{
    // Register S3 bucket
    // bucketName: Your S3 bucket name
    // awsAccessKeyId: Your AWS access key
    // awsSecretAccessKey: Your AWS secret key
    // bucketRegion: Your AWS region (e.g., us-east-1)
}
```

#### Step 3: Configure appsettings.json

```json
{
  "AWSSettings": {
    "BucketName": "your-bucket-name",
    "AccessKeyId": "your-access-key",
    "SecretAccessKey": "your-secret-key",
    "BucketRegion": "us-east-1"
  }
}
```

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:5000/api/AmazonS3Provider/AmazonS3FileOperations',
    downloadUrl: 'http://localhost:5000/api/AmazonS3Provider/AmazonS3Download',
    uploadUrl: 'http://localhost:5000/api/AmazonS3Provider/AmazonS3Upload',
    getImageUrl: 'http://localhost:5000/api/AmazonS3Provider/AmazonS3GetImage'
  };
}
```

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download | ✅ Search

### Documentation

**AWS S3 Documentation:** [Create and configure S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-configure-bucket.html)

### Repository

**GitHub:** [amazon-s3-aspcore-file-provider](https://github.com/SyncfusionExamples/amazon-s3-aspcore-file-provider)

---

## SharePoint File Provider

### Overview

The SharePoint file provider enables access to files and folders within Microsoft SharePoint document libraries, integrating with Microsoft 365 environments.

### Prerequisites

- Microsoft 365 Account
- SharePoint Site
- Azure App Registration
- Tenant ID, Client ID, Client Secret
- User Site Name and Drive ID

### Setup

#### Step 1: Create Azure App Registration

1. Navigate to Azure Portal
2. Go to Azure Active Directory
3. Create new App Registration
4. Note: Tenant ID, Client ID, Client Secret

#### Step 2: Clone Repository

```bash
git clone https://github.com/SyncfusionExamples/sharepoint-aspcore-file-provider sharepoint-aspcore-file-provider
cd sharepoint-aspcore-file-provider
```

#### Step 3: Configure appsettings.json

```json
{
  "SharePointSettings": {
    "TenantId": "your-tenant-id",
    "ClientId": "your-client-id",
    "ClientSecret": "your-client-secret",
    "UserSiteName": "your-site-name",
    "UserDriveId": "your-drive-id"
  }
}
```

#### Step 4: SharePoint Controller Configuration

```csharp
// In SharePointController.cs
// The controller automatically loads credentials from appsettings.json
// No additional configuration needed
public class SharePointController : ControllerBase
{
    // Controller automatically initialized with settings
}
```

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:5000/api/SharePointProvider/SharePointFileOperations',
    downloadUrl: 'http://localhost:5000/api/SharePointProvider/SharePointDownload',
    uploadUrl: 'http://localhost:5000/api/SharePointProvider/SharePointUpload',
    getImageUrl: 'http://localhost:5000/api/SharePointProvider/SharePointGetImage'
  };
}
```

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download | ✅ Search

### Repository

**GitHub:** [sharepoint-aspcore-file-provider](https://github.com/SyncfusionExamples/sharepoint-aspcore-file-provider)

---

## File Transfer Protocol Provider

### Overview

The FTP file provider allows management of files on FTP servers, enabling integration with legacy FTP-based storage systems.

### Prerequisites

- FTP Server
- FTP Host Name
- FTP Username
- FTP Password

### Setup

#### Step 1: Clone Repository

```bash
git clone https://github.com/SyncfusionExamples/ftp-aspcore-file-provider ftp-aspcore-file-provider
cd ftp-aspcore-file-provider
```

#### Step 2: Configure FTP Connection

```csharp
// In FTPProviderController.cs
public void SetFTPConnection(string hostName, string userName, string password)
{
    // Configure FTP server connection
    // hostName: FTP server address
    // userName: FTP login username
    // password: FTP login password
}
```

#### Step 3: Configure appsettings.json

```json
{
  "FTPSettings": {
    "HostName": "ftp.example.com",
    "UserName": "ftp-username",
    "Password": "ftp-password",
    "Port": 21
  }
}
```

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:5000/api/FTPProvider/FTPFileOperations',
    downloadUrl: 'http://localhost:5000/api/FTPProvider/FTPDownload',
    uploadUrl: 'http://localhost:5000/api/FTPProvider/FTPUpload',
    getImageUrl: 'http://localhost:5000/api/FTPProvider/FTPGetImage'
  };
}
```

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download

### Repository

**GitHub:** [ftp-aspcore-file-provider](https://github.com/SyncfusionExamples/ftp-aspcore-file-provider)

---

## SQL Database File System Provider

### Overview

The SQL database file provider stores the file system structure in a SQL database, with operations performed on ID basis rather than path basis.

### Prerequisites

- SQL Server or LocalDB
- Database with FileManager schema
- Connection string

### Setup

#### Step 1: Clone Repository

```bash
git clone https://github.com/SyncfusionExamples/sql-server-database-aspcore-file-provider sql-server-database-aspcore-file-provider
cd sql-server-database-aspcore-file-provider
```

#### Step 2: Configure Connection String

```json
{
  "ConnectionStrings": {
    "FileManagerConnection": "Data Source=(LocalDB)\\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\\App_Data\\FileManager.mdf;Integrated Security=True;Connect Timeout=30"
  }
}
```

#### Step 3: Set SQL Connection

```csharp
// In SQLProviderController.cs
public void SetSQLConnection(string name, string tableName, string tableID)
{
    // Configure SQL connection
    // name: Connection string name from appsettings.json
    // tableName: Table name storing file system
    // tableID: Primary key column name
}
```

#### Step 4: Use Pre-configured Database

Download the pre-configured [FileManager.mdf](https://github.com/SyncfusionExamples/sql-server-database-aspcore-file-provider/blob/master/App_Data/FileManager.mdf) file

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:5000/api/SQLProvider/SQLFileOperations',
    downloadUrl: 'http://localhost:5000/api/SQLProvider/SQLDownload',
    uploadUrl: 'http://localhost:5000/api/SQLProvider/SQLUpload',
    getImageUrl: 'http://localhost:5000/api/SQLProvider/SQLGetImage'
  };
}
```

### Database Schema

```sql
CREATE TABLE FileManager (
    id INT PRIMARY KEY IDENTITY(1,1),
    parentId INT,
    name NVARCHAR(255),
    isFile BIT,
    size BIGINT,
    dateModified DATETIME,
    type NVARCHAR(50),
    hasChild BIT
)
```

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download | ✅ Search

### Repository

**GitHub:** [sql-server-database-aspcore-file-provider](https://github.com/SyncfusionExamples/sql-server-database-aspcore-file-provider)

---

## Node.js File System Provider

### Overview

The Node.js file provider enables file management through a Node.js server, providing a lightweight runtime alternative to ASP.NET Core.

### Prerequisites

- Node.js runtime (v12 or higher)
- npm package manager

### Setup

#### Option 1: Install via NPM Package

```bash
npm install @syncfusion/ej2-filemanager-node-filesystem
cd node_modules/@syncfusion/ej2-filemanager-node-filesystem
npm install
```

#### Option 2: Clone from GitHub

```bash
git clone https://github.com/SyncfusionExamples/ej2-filemanager-node-filesystem node-filesystem-provider
cd node-filesystem-provider
npm install
```

#### Step 3: Set Root Directory and Port

```bash
# Windows
set PORT=3000 && node filesystem-server.js -d D:/Projects

# Linux/Mac
PORT=3000 node filesystem-server.js -d /home/user/projects
```

Or configure in package.json:

```json
{
  "scripts": {
    "start": "node filesystem-server.js -d D:/Projects"
  }
}
```

#### Step 4: Run Server

```bash
npm start
```

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:8090/',
    downloadUrl: 'http://localhost:8090/Download',
    uploadUrl: 'http://localhost:8090/Upload',
    getImageUrl: 'http://localhost:8090/GetImage'
  };
}
```

### Default Configuration

- **Default Port:** 8090
- **Default Root Directory:** C:/Users (Windows), /home (Linux)

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download | ✅ Search

### Repository

**NPM:** [@syncfusion/ej2-filemanager-node-filesystem](https://www.npmjs.com/package/@syncfusion/ej2-filemanager-node-filesystem)
**GitHub:** [ej2-filemanager-node-filesystem](https://github.com/SyncfusionExamples/ej2-filemanager-node-filesystem)

---

## Google Drive File System Provider

### Overview

The Google Drive file provider integrates with Google Drive API, enabling file management directly within Google Drive accounts using OAuth 2.0 authentication.

### Prerequisites

- Google Account
- Google Cloud Console Project
- OAuth 2.0 Client ID and Secret
- Google Drive API enabled

### Setup

#### Step 1: Create Google Cloud Project

1. Navigate to [Google Cloud Console](https://console.cloud.google.com/)
2. Create new project
3. Enable Google Drive API
4. Create OAuth 2.0 credentials (Desktop application)
5. Download credentials JSON file

#### Step 2: Clone Repository

```bash
git clone https://github.com/SyncfusionExamples/google-drive-aspcore-file-provider google-drive-aspcore-file-provider
cd google-drive-aspcore-file-provider
```

#### Step 3: Configure Credentials

Copy the downloaded credentials JSON to both locations:

```
EJ2GoogleDriveFileProvider/credentials/client_secret.json
GoogleOAuth2.0Base/credentials/client_secret.json
```

#### Step 4: Configure appsettings.json

```json
{
  "GoogleDriveSettings": {
    "ClientId": "your-client-id",
    "ClientSecret": "your-client-secret",
    "RootFolderId": "root"
  }
}
```

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:5000/api/GoogleDriveProvider/GoogleDriveFileOperations',
    downloadUrl: 'http://localhost:5000/api/GoogleDriveProvider/GoogleDriveDownload',
    uploadUrl: 'http://localhost:5000/api/GoogleDriveProvider/GoogleDriveUpload',
    getImageUrl: 'http://localhost:5000/api/GoogleDriveProvider/GoogleDriveGetImage'
  };
}
```

### Authentication Flow

1. User accesses File Manager
2. Redirected to Google login
3. User authorizes application
4. Application receives access token
5. File operations proceed with authentication

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download | ✅ Search

### Documentation

**Google Drive API:** [Google Drive API Reference](https://developers.google.com/drive/api/v3/reference/)
**OAuth 2.0:** [OAuth 2.0 for JavaScript](https://developers.google.com/identity/protocols/oauth2/javascript-implicit-flow)

### Repository

**GitHub:** [google-drive-aspcore-file-provider](https://github.com/SyncfusionExamples/google-drive-aspcore-file-provider)

---

## Firebase Realtime Database Provider

### Overview

The Firebase Realtime Database provider stores file system metadata in Firebase's cloud database as JSON, enabling serverless file system management.

### Prerequisites

- Firebase Account
- Firebase Project
- Service Account Key
- Firebase Realtime Database

### Setup

#### Step 1: Create Firebase Project

1. Navigate to [Firebase Console](https://console.firebase.google.com/)
2. Create new project
3. Create Realtime Database
4. Create JSON database structure (see below)

#### Step 2: Create Database Structure

```json
{
  "Files": [
    {
      "caseSensitive": false,
      "dateCreated": "8/22/2019 5:17:55 PM",
      "dateModified": "8/22/2019 5:17:55 PM",
      "filterId": "0/",
      "filterPath": "/",
      "hasChild": false,
      "id": "5",
      "isFile": false,
      "isRoot": true,
      "name": "Music",
      "parentId": "0",
      "selected": false,
      "showHiddenItems": false,
      "size": 0,
      "type": "folder"
    },
    {
      "caseSensitive": false,
      "dateCreated": "8/22/2019 5:18:03 PM",
      "dateModified": "8/22/2019 5:18:03 PM",
      "filterId": "0/",
      "filterPath": "/",
      "hasChild": false,
      "id": "6",
      "isFile": false,
      "isRoot": true,
      "name": "videos",
      "parentId": "0",
      "selected": false,
      "showHiddenItems": false,
      "size": 0,
      "type": ""
    }
  ]
}
```

#### Step 3: Import JSON to Firebase

1. Open Firebase Database
2. Click action menu (⋮)
3. Select "Import JSON"
4. Upload the JSON file from Step 2

#### Step 4: Configure Firebase Rules

```json
{
  "rules": {
    ".read": "auth!=null",
    ".write": "auth!=null"
  }
}
```

#### Step 5: Generate Service Account Key

1. Go to Firebase Project Settings
2. Navigate to Service Account tab
3. Click "Generate new private key"
4. Save the JSON file as `access_key.json`

#### Step 6: Clone Repository

```bash
git clone https://github.com/SyncfusionExamples/firebase-realtime-database-aspcore-file-provider firebase-provider
cd firebase-provider
```

#### Step 7: Configure appsettings.json

```json
{
  "FirebaseSettings": {
    "ApiUrl": "https://your-project.firebaseio.com",
    "RootNode": "Files",
    "ServiceAccountKeyPath": "FirebaseRealtimeDBHelper/access_key.json"
  }
}
```

#### Step 8: Register Firebase in Controller

```csharp
// In FirebaseProviderController.cs
this.operation.RegisterFirebaseRealtimeDB(
    "{your-api-url}",
    "Files",
    hostingEnvironment.ContentRootPath + "\\FirebaseRealtimeDBHelper\\access_key.json"
);
```

### Angular Component Configuration

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<ejs-filemanager [ajaxSettings]='ajaxSettings'></ejs-filemanager>`
})
export class AppComponent {
  public ajaxSettings = {
    url: 'http://localhost:5000/api/FirebaseProvider/FirebaseRealtimeFileOperations',
    downloadUrl: 'http://localhost:5000/api/FirebaseProvider/FirebaseRealtimeDownload',
    uploadUrl: 'http://localhost:5000/api/FirebaseProvider/FirebaseRealtimeUpload',
    getImageUrl: 'http://localhost:5000/api/FirebaseProvider/FirebaseRealtimeGetImage'
  };
}
```

### Supported Operations

✅ Create folder | ✅ Copy | ✅ Move | ✅ Delete | ✅ Upload | ✅ Download | ✅ Search

### Documentation

**Firebase Docs:** [Firebase Realtime Database](https://firebase.google.com/docs/database)
**Firebase Security:** [Firebase Database Security Rules](https://firebase.google.com/docs/database/security)

### Repository

**GitHub:** [firebase-realtime-database-aspcore-file-provider](https://github.com/SyncfusionExamples/firebase-realtime-database-aspcore-file-provider)

---

## Comparison Table

### Provider Features Comparison

| Feature | Physical | Azure | S3 | SharePoint | FTP | SQL | Node.js | Google Drive | Firebase |
|---------|----------|-------|-----|-----------|-----|-----|---------|-------------|----------|
| **On-Premise** | ✅ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ |
| **Cloud-Based** | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | ✅ |
| **Enterprise** | ❌ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |
| **Scalability** | Low | High | High | High | Low | Medium | Medium | High | High |
| **Setup Complexity** | Low | Medium | Medium | High | Low | Medium | Low | High | Medium |
| **Cost** | Low | Medium | Medium | High | Low | Low | Low | Free | Free |
| **Authentication** | Windows | Azure | AWS | OAuth | Basic | SQL | None | OAuth | Firebase |
| **Permissions** | OS | RBAC | IAM | SharePoint | FTP | SQL | OS | Google | Firebase |

---

## Quick Selection Guide

### Choose Physical Provider If:
- ✅ Running on-premise server
- ✅ Need simplest setup
- ✅ Managing local server files
- ✅ Low budget

### Choose Azure Provider If:
- ✅ Using Azure infrastructure
- ✅ Need enterprise scalability
- ✅ Want managed service
- ✅ Need RBAC and compliance

### Choose AWS S3 Provider If:
- ✅ Using AWS infrastructure
- ✅ Need object storage
- ✅ Want high availability
- ✅ Need global distribution

### Choose SharePoint Provider If:
- ✅ Using Microsoft 365
- ✅ Need enterprise features
- ✅ Want SharePoint integration
- ✅ Need compliance features

### Choose SQL Provider If:
- ✅ Have existing database
- ✅ Want custom file schema
- ✅ Need database-backed storage
- ✅ Want relational approach

### Choose Firebase Provider If:
- ✅ Need serverless solution
- ✅ Starting small project
- ✅ Want real-time updates
- ✅ Free tier available

### Choose Google Drive Provider If:
- ✅ Using Google Workspace
- ✅ Want consumer-grade integration
- ✅ Need simple sharing
- ✅ Existing Google ecosystem

---

## Best Practices

1. **Choose Based on Infrastructure**
   - Already using Azure? → Azure Provider
   - Already using AWS? → S3 Provider
   - On-premise only? → Physical Provider

2. **Consider Security Requirements**
   - Enterprise security? → Azure, SharePoint
   - Need OAuth? → Google Drive, Firebase
   - Simple auth? → FTP, Physical

3. **Evaluate Scalability**
   - Large files (GB+)? → S3, Azure
   - Thousands of files? → SQL, Firebase
   - Limited users? → Physical, FTP

4. **Plan for Cost**
   - Free tier? → Firebase (first year)
   - Minimal cost? → Physical, Node.js
   - Enterprise budget? → Azure, SharePoint

5. **Maintenance Burden**
   - No maintenance? → Azure, S3, Firebase
   - Self-managed? → Physical, FTP, SQL
   - Hybrid? → Node.js

---

## API Endpoint Reference

All providers expose these standard endpoints:

```
POST   /api/[Provider]/[Operation]FileOperations  - File operations
GET    /api/[Provider]/[Operation]Download        - Download files
POST   /api/[Provider]/[Operation]Upload          - Upload files
GET    /api/[Provider]/[Operation]GetImage        - Get thumbnails
```

---

## Related Documentation

- [Custom File Provider](custom-file-provider.md)
- [Authentication and Custom Values](authentication-and-custom-values.md)
- [File Operations](file-operations.md)

---

**Next:** Choose a provider and refer to its setup section above, then configure your Angular FileManager with the appropriate `ajaxSettings`.
