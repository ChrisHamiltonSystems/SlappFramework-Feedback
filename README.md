# SlappFramework-Feedback
Community feedback, issues, and discussions for SlappFramework

- **Project Website**: https://www.slappframework.com

# Welcome to SlappFramework!

> **"Data as valid as your staging table, GUARANTEED!"**

SlappFramework is a constraint-driven, SQL-centric web framework for building bulk data management interfaces. Your database constraints ARE your validators - no validation code needed.

## Quick Start (3 Steps)

### 1. Configure Database
Update `appsettings.json` with your SQL Server connection string:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost;Database=YourDatabase;Integrated Security=true;TrustServerCertificate=true;"
}
```

### 2. Install Database Infrastructure
Execute `SqlScripts/CreateBulkOpsInfrastructure.sql` against your database.

This creates the `ZZ_SlappFramework` schema with all necessary metadata tables, validation stored procedures, and scaffolding tools.

### 3. Scaffold Your First Feature
Open SQL Server Management Studio and run:

```sql
-- Example: Create a bulk insert feature for your "Products" table
EXEC ZZ_SlappFramework.ScaffoldAn_INSERT_Feature
    @DomainSchemaName = 'dbo',
    @DomainTableName = 'Products'

-- Or a bulk update feature
EXEC ZZ_SlappFramework.ScaffoldAn_UPDATE_Feature
    @DomainSchemaName = 'dbo',
    @DomainTableName = 'Products'
```

Run the app and your features appear in the menu automatically!

## What You Get

**Six bulk operation workflows out-of-the-box:**
- **insertViaForm** - Single-record form entry
- **insertViaExcel** - Download template → Edit offline → Upload
- **insertViaHandsontable** - Excel-like grid editing (inline)
- **updateViaForm** - Edit single records
- **updateViaExcel** - Select rows → Download → Edit → Upload
- **updateViaHandsontable** - Bulk edit in grid

**All workflows include:**
- Automatic constraint validation (your staging table = your validator)
- Foreign key dropdown support
- Rich error messages
- Transaction-based processing
- Excel import/export with FK dropdown sheets

## The Core Concept

Instead of writing validation code, design proper table constraints:

```sql
CREATE TABLE dbo.Products (
    ProductName NVARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) CHECK (Price > 0),
    CategoryID INT NOT NULL
        FOREIGN KEY REFERENCES dbo.Categories(CategoryID),
    -- Scaffolding generates a staging table to enforce ALL of this automatically
)
```

SlappFramework bulk-copies your data to the staging table, SQL Server enforces constraints, and the framework collects detailed error messages. Only valid data proceeds to your domain tables.

**Result:** Zero validation code. Data as valid as your staging table. Guaranteed.

## Project Structure

```
├── Controllers/
│   ├── HomeController.cs      # Main UI controller
│   └── DataController.cs      # REST API endpoints
├── Services/
│   ├── BulkOpsHelper.cs       # Core validation engine
│   ├── DataService.cs         # SQL Server wrapper
│   ├── ExcelTemplateGenerator.cs
│   └── ExcelImporter.cs
├── Models/                     # Data models and DTOs
├── Views/Home/                 # Razor views
├── wwwroot/                    # Static assets
└── SqlScripts/
    └── CreateBulkOpsInfrastructure.sql
```

## Next Steps

1. Run the app and explore the example features (if using a sample database)
2. Review `Docs_Misc/SlappFramework-LLM-Context.md` for architecture details
3. Scaffold features for your own domain tables
4. Customize domain processing sprocs for business logic

## Support & Feedback
- **Docs:** https://www.slappframework.com
---

**This is an alpha/preview release.** Feedback welcome!
