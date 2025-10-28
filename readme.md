# Azure Data Factory - Medallion Architecture

A production-ready Azure Data Factory (ADF) implementation of the Medallion Architecture (Bronze-Silver-Gold) for enterprise data lake processing on Azure.

## 🏗️ Architecture Overview

This project implements a complete medallion architecture pattern using Azure Data Factory, organizing data into three distinct layers:

```
Bronze Layer (Raw)  →  Silver Layer (Refined)  →  Gold Layer (Curated)
     ↓                        ↓                          ↓
  Raw Data              Cleaned Data              Business-Ready Data
  As-Is Storage         Validated Data            Aggregated Metrics
  Full History          Deduplication             Optimized for BI
```

### Layer Breakdown

**🥉 Bronze Layer (Raw Zone)**
- Ingests raw data from various source systems
- Preserves data in its original format
- Append-only storage for complete audit trail
- Minimal transformations applied

**🥈 Silver Layer (Refined Zone)**
- Cleanses and validates bronze data
- Applies data quality rules
- Standardizes data formats and schemas
- Removes duplicates and handles nulls

**🥇 Gold Layer (Curated Zone)**
- Business-ready, aggregated datasets
- Optimized for analytics and reporting
- Conforms to dimensional models
- Powers dashboards and BI tools

## 📁 Project Structure

```
├── dataflow/              # Data transformation flows
│   └── (transformation logic for each layer)
├── dataset/               # Dataset definitions
│   └── (source and sink configurations)
├── factory/               # Factory configuration
│   └── (ADF settings and metadata)
├── linkedService/         # Connection configurations
│   ├── AzureDataLakeStorage
│   └── AzureBlobStorageForSnowflake
├── pipeline/              # Orchestration pipelines
│   └── earlyDim (dimension loading)
└── publish_config.json    # Publishing configuration
```

## 🚀 Getting Started

### Prerequisites

- **Azure Subscription** with appropriate permissions
- **Azure Data Factory** instance
- **Azure Data Lake Storage Gen2** account
- **Azure Key Vault** (recommended for secrets)
- **Azure DevOps or GitHub** for CI/CD (optional)

### Setup Instructions

1. **Clone the Repository**
```bash
git clone https://github.com/SiddhiRavindra/your-repo-name.git
cd your-repo-name
```

2. **Configure Linked Services**
   - Navigate to the `linkedService/` folder
   - Update connection strings for:
     - Azure Data Lake Storage
     - Azure Blob Storage
     - Any source systems
   - Store sensitive credentials in Azure Key Vault

3. **Deploy to Azure Data Factory**
   ```bash
   # Using Azure CLI
   az datafactory factory create \
     --resource-group <resource-group> \
     --factory-name <factory-name> \
     --location <location>
   ```

4. **Import ADF Resources**
   - Use ADF Studio UI or ARM templates
   - Import pipelines, datasets, and dataflows
   - Validate all linked service connections

5. **Configure Parameters**
   - Set environment-specific parameters
   - Update storage paths for each layer
   - Configure trigger schedules

## 🔄 Pipeline Architecture

### Data Flow Process

1. **Ingestion Pipeline**
   - Extracts data from source systems
   - Loads into Bronze layer (ADLS)
   - Maintains metadata and lineage

2. **Transformation Pipeline**
   - Processes Bronze to Silver
   - Data quality checks
   - Schema validation
   - Deduplication logic

3. **Aggregation Pipeline**
   - Processes Silver to Gold
   - Business logic application
   - Dimensional modeling
   - Creates analytics-ready datasets

### Early Dimension Pipeline

The `earlyDim` pipeline handles dimension table loading:
- Extracts dimension data from sources
- Applies SCD (Slowly Changing Dimension) logic
- Updates dimension tables in Gold layer
- Maintains historical tracking

## 📊 Data Flows

### Bronze → Silver Transformation
- **Data Cleansing**: Remove invalid records
- **Type Casting**: Ensure correct data types
- **Standardization**: Consistent naming conventions
- **Deduplication**: Remove duplicate records
- **Null Handling**: Apply default values or filters

### Silver → Gold Transformation
- **Aggregations**: Sum, count, average metrics
- **Joins**: Combine related datasets
- **Filtering**: Apply business rules
- **Partitioning**: Optimize for query performance
- **Dimensional Modeling**: Star/snowflake schemas

## 🔗 Linked Services

### Azure Data Lake Storage
- **Purpose**: Primary storage for all three layers
- **Structure**: Separate containers for Bronze/Silver/Gold
- **Configuration**: Hierarchical namespace enabled

### Azure Blob Storage for Snowflake
- **Purpose**: Integration with Snowflake data warehouse
- **Use Case**: External stage for Snowflake loading
- **Benefits**: Efficient bulk data transfer

## 🛠️ Configuration

### Environment Variables
```json
{
  "bronzeContainer": "bronze",
  "silverContainer": "silver",
  "goldContainer": "gold",
  "storageAccount": "your-storage-account",
  "dataLakeUri": "https://youraccount.dfs.core.windows.net"
}
```

### Pipeline Parameters
- `sourceSystem`: Source data system identifier
- `entityName`: Table or entity being processed
- `loadDate`: Date partition for data processing
- `incrementalLoad`: Boolean for full/incremental loads

## 📈 Monitoring & Logging

### Built-in ADF Monitoring
- Pipeline run history and status
- Activity-level execution metrics
- Data flow performance statistics
- Error logs and troubleshooting

### Recommended Practices
- Set up Azure Monitor alerts
- Configure Log Analytics workspace
- Create custom dashboards in Azure Portal
- Implement data quality metrics

## 🔒 Security Best Practices

- **Managed Identity**: Use for service-to-service authentication
- **Key Vault Integration**: Store all secrets and connection strings
- **RBAC**: Implement role-based access control
- **Network Security**: Configure private endpoints
- **Data Encryption**: Enable at-rest and in-transit encryption

## 🧪 Testing

### Unit Testing
- Test individual data flow transformations
- Validate data quality rules
- Check schema mappings

### Integration Testing
- End-to-end pipeline execution
- Cross-layer data validation
- Performance benchmarking

## 🚦 CI/CD Pipeline

### Deployment Strategy
1. Development → Testing → Production
2. ARM template-based deployments
3. Automated testing in lower environments
4. Approval gates for production

### Azure DevOps Integration
```yaml
trigger:
  branches:
    include:
      - main

stages:
  - stage: Build
    jobs:
      - job: ValidateADF
  - stage: Deploy
    jobs:
      - job: DeployToProduction
```

## 📝 Best Practices

### Data Organization
- Partition data by date for efficient querying
- Use Parquet format for optimal performance
- Implement data retention policies
- Archive historical data appropriately

### Pipeline Design
- Keep pipelines modular and reusable
- Use parameters for flexibility
- Implement error handling and retry logic
- Log detailed execution metadata

### Performance Optimization
- Use data flow debug for testing
- Optimize partition counts
- Leverage integration runtime scaling
- Monitor and tune data flow performance


## 📚 Additional Resources

- [Azure Data Factory Documentation](https://docs.microsoft.com/azure/data-factory/)
- [Medallion Architecture Best Practices](https://docs.databricks.com/lakehouse/medallion.html)
- [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)
- [Data Flow Transformations](https://docs.microsoft.com/azure/data-factory/data-flow-transformation-overview)

## 📧 Contact

**Project Maintainer**: SiddhiRavindra

For questions, issues, or suggestions, please open an issue in this repository.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**Built with Azure Data Factory** | **Medallion Architecture Implementation**
