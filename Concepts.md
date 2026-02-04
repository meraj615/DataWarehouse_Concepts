#### Basic DataWare House Concepts for the Data Engineer #########

### `Data Warehouse`
**Purpose:** Acts as a centralized repository that stores structured, cleaned, and integrated data from multiple sources to support reporting, analytics, and business decision-making.
**Key Idea:** Optimized for read-heavy analytical queries, not day-to-day transactions.


### `OLTP (Online Transaction Processing)`
**Purpose:** Handles day-to-day business operations such as inserts, updates, and deletes.
**Key Idea:** Focuses on speed, accuracy, and concurrency for transactional workloads.


### `OLAP (Online Analytical Processing)`
**Purpose:** Supports complex queries, aggregations, and trend analysis for reporting and analytics.
**Key Idea:** Designed for historical analysis and decision support, not real-time transactions.


### `Subject-Oriented`
**Purpose:** Organizes data around major business entities like Customers, Sales, Products, rather than application processes.
**Key Idea:** Makes data easier for business users to understand and analyze.


### `Integrated`
**Purpose:** Combines data from multiple heterogeneous sources (databases, files, APIs) into a unified format.
**Key Idea:** Ensures consistent naming conventions, data types, and business definitions.

### `Time-Variant`
**Purpose:** Stores historical data with a time dimension to track changes over long periods.
**Key Idea:** Enables trend analysis, year-over-year comparisons, and historical reporting.

### `Non-Volatile`
**Purpose:** Data is stable and primarily read-only after loading.
**Key Idea:** Once data is loaded, it is not frequently updated or deleted, ensuring data consistency.

#### Data Warehouse Architecture #########

### `Source Systems`
**Purpose:** Provide raw operational and external data to the data warehouse pipeline.
**Includes:** OLTP databases, flat files (CSV/Excel), APIs, third-party tools, and external data feeds.
**Key Idea:** This is where data is generated, not optimized for analytics.

### `Staging Area`
**Purpose:** Acts as a temporary holding zone for incoming raw data before transformation.
**Key Idea:** Isolates source systems from the warehouse and supports data validation, cleansing, and auditing.


### `ETL (Extract, Transform, Load)`
**Purpose:** Extracts data from source systems, transforms it into a consistent format, and loads it into the warehouse.
**Key Idea:** Transformation happens before loading, commonly used in traditional on-prem systems.

### `ELT (Extract, Load, Transform)`
**Purpose:** Extracts data, loads it directly into the warehouse, and performs transformations inside the warehouse.
**Key Idea:** Leverages cloud data warehouse power (e.g., Snowflake, BigQuery) for scalable transformations.

### `Data Warehouse`
**Purpose:** Serves as the centralized, integrated, and historical data store for analytics.
**Key Idea:** Optimized for complex queries, joins, and aggregations across large datasets.


### `Data Mart`
**Purpose:** Provides a focused, department-specific subset of warehouse data (Sales, Finance, HR).
**Key Idea:** Improves performance and usability for business users with targeted analytics.


### `Presentation Layer`
**Purpose:** Enables end users to analyze and visualize data.
**Includes:** BI tools, dashboards, reports, and ad-hoc queries.
**Key Idea:** Converts raw data into actionable business insights.

#### Data Modeling #########

### `Fact Table`
**Purpose:** Stores measurable, quantitative business data used for analysis.
**Includes:** Sales amount, quantity sold, revenue, transaction count.
**Key Idea:** Contains foreign keys to dimensions and numeric metrics for aggregation.

### `Dimension Table`
**Purpose:** Holds descriptive attributes that provide context to fact data.
**Includes:** Customer name, product category, region, date.
**Key Idea:** Used to filter, group, and label facts during analysis.


### `Surrogate Key`
**Purpose:** A system-generated unique identifier for dimension records.
**Key Idea:** Improves performance and handles slowly changing dimensions independent of business keys.

### `Natural Key`
**Purpose:** A business-defined identifier coming from source systems.
**Examples:** Customer ID, Employee ID, Product Code.
**Key Idea:** Represents real-world identity, but may change or be reused.

#### Schema Types #########

### `Star Schema`
**Purpose:** Organizes data with one central fact table connected to multiple dimension tables.
**Key Idea:** Simple design with fewer joins, resulting in faster query performance and easier understanding.
**Best For:** Reporting, BI tools, and beginner-friendly analytics models.


### `Snowflake Schema`
**Purpose:** Extends the star schema by normalizing dimension tables into multiple related tables.
**Key Idea:** Reduces data redundancy but introduces more joins during queries.
**Best For:** Complex dimensions and storage-optimized environments.


### `Galaxy Schema`
**Purpose:** Supports multiple fact tables that share common dimension tables.
**Key Idea:** Enables analysis across different business processes (e.g., Sales, Inventory, Shipping).
**Best For:** Large enterprise data warehouses with multiple subject areas.

### `Flat Schema`
**Purpose:** Stores all data in a single denormalized table.
**Key Idea:** Easy to query but hard to maintain and scale as data grows.
**Best For:** Small datasets, quick analysis, or staging/temporary reporting layers.

#### Slowly Changing Dimensions (SCD) #########

### `SCD Type 0`
**Purpose:** Prevents any changes to existing dimension data.
**Key Idea:** Original values are preserved forever, even if the source changes.
**Use Case:** Attributes that should never change (e.g., Date of Birth).


### `SCD Type 1`
**Purpose:** Updates dimension data by overwriting old values.
**Key Idea:** No history is maintained; only the latest value is stored.
**Use Case:** Correcting data errors such as typo fixes or formatting changes.


### `SCD Type 2`
**Purpose:** Tracks full historical changes by creating a new row for each change.
**Key Idea:** Uses effective dates, current flags, or version numbers to preserve history.
**Use Case:** When historical accuracy is critical (e.g., customer address changes).

### `SCD Type 3`
**Purpose:** Maintains limited history by adding new columns for previous values.
**Key Idea:** Stores only a fixed number of historical states.
**Best For:** Comparing current vs previous values (e.g., current vs prior region).


### `SCD Type 4`
**Purpose:** Separates current data and historical data into different tables.
**Key Idea:** Improves query performance on current records while preserving history elsewhere.
**Best For:** Large dimensions with heavy history tracking requirements.

#### Data Quality #########

### `Accuracy`
**Purpose:** Ensures data correctly represents real-world values.
**Key Idea:** Data must be free from errors and reflect the true business state.


### `Completeness`
**Purpose:** Confirms all required data fields are populated.
**Key Idea:** Critical attributes should have no missing or null values where they are expected.


### `Consistency`
**Purpose:** Maintains uniform data values across multiple systems and reports.
**Key Idea:** The same data should mean and appear the same everywhere.

### `Timeliness`
**Purpose:** Ensures data is available when it is needed.
**Key Idea:** Late data can be as harmful as incorrect data for decision-making.


### `Validity`
**Purpose:** Verifies data follows defined business rules and formats.
**Key Idea:** Values must conform to constraints, ranges, and data types.


### `Uniqueness`
**Purpose:** Guarantees that each record appears only once.
**Key Idea:** Prevents duplicate records, especially in keys and identifiers.


#### Performance Optimization #########

### `Indexing`
**Purpose:** Improves query performance by enabling faster data lookup.
**Key Idea:** Reduces full table scans by allowing the database to locate rows efficiently.
**Use Case:** Frequently filtered or joined columns.


### `Partitioning`
**Purpose:** Divides large tables into smaller, manageable segments
**Key Idea:** Queries scan only relevant partitions, improving performance and maintenance.
**Use Case:** Time-based data such as daily, monthly, or yearly records.


### `Clustering`
**Purpose:** Physically organizes data based on column values.
**Key Idea:** Improves performance by reducing data scanned during queries.
**Use Case:** Columns commonly used in filters or range queries.

### `Timeliness`
**Purpose:** Ensures data is available when it is needed.
**Key Idea:** Late data can be as harmful as incorrect data for decision-making.


### `Materialized View`
**Purpose:** Stores summarized data at a higher level of granularity.
**Key Idea:** Avoids recalculating complex joins and aggregations repeatedly.
**Use Case:** Frequently accessed dashboards and heavy analytical queries.


### `Aggregates`
**Purpose:** Guarantees that each record appears only once.
**Key Idea:** Reduces query workload by using pre-summarized metrics.
**Use Case:** Reporting scenarios like daily sales, monthly revenue, or yearly trends.


#### Data Governance & Security #########

### `Data Lineage`
**Purpose:** Tracks the complete flow of data from source to destination.
**Key Idea:** Provides visibility into where data comes from, how it changes, and where it is used.
**Use Case:** Impact analysis, debugging, and regulatory compliance.


### `Data Ownership`
**Purpose:** Defines accountability for data assets across the organization.
**Key Idea:** Assigns clear responsibility for data quality, access, and maintenance.
**Use Case:** Prevents ambiguity when issues or changes arise.


### `Access Control`
**Purpose:** Restricts data access based on roles and responsibilities.
**Key Idea:** Implements role-based access control (RBAC) to protect sensitive data.
**Use Case:** Ensuring users only see data they are authorized to view.


### `Masking`
**Purpose:** Protects sensitive information by obscuring or anonymizing data.
**Key Idea:** Prevents exposure of PII/PHI while allowing analytical usage.
**Use Case:** Compliance with regulations like GDPR, HIPAA.


### `Auditing`
**Purpose:** Monitors and records data access and changes.
**Key Idea:** Creates a traceable audit trail for security and compliance reviews.
**Use Case:** Detecting unauthorized access and meeting regulatory requirements.


#### Most Important Topics To Cover #########

### `Fact Table`
### `Dimension Table`
### `Star Schema vs Snowflake Schema`
### `SCD Types`
### `ETL vs ELT`
### `Incremental Load`
### `OLTP vs OLAP`







