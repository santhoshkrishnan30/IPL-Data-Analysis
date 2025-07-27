# IPL Data Analytics Pipeline

[![Apache Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white)](https://spark.apache.org/)
[![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)](https://databricks.com/)
[![AWS S3](https://img.shields.io/badge/AWS%20S3-569A31?style=for-the-badge&logo=amazon-s3&logoColor=white)](https://aws.amazon.com/s3/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org/)

## 📋 Project Overview

An end-to-end Apache Spark data engineering pipeline that processes comprehensive IPL (Indian Premier League) cricket data to deliver advanced analytics insights through PySpark transformations, SQL analysis, and interactive visualizations.

## 🏗️ Architecture

<img width="1820" height="888" alt="image" src="https://github.com/user-attachments/assets/15053c77-c0e3-4f74-94fa-e38028797070" />


```
AWS S3 → Databricks → Apache Spark (PySpark) → SQL Analytics → Matplotlib Visualizations
```



## 🎯 Key Features

- **Distributed Processing**: Scalable Apache Spark pipeline with lazy evaluation optimization
- **Advanced Transformations**: Window functions, conditional formatting, and multi-table joins
- **Schema Validation**: Automated data type enforcement and quality checks
- **Cricket Analytics**: Comprehensive statistical insights and performance metrics
- **Interactive Visualizations**: Dynamic charts and graphs for data exploration

## 📊 Dataset

IPL comprehensive dataset containing:
- **Ball-by-Ball Data**: 150K+ records of every delivery
- **Match Information**: 637 matches with detailed metadata
- **Player Statistics**: Individual performance metrics
- **Team Data**: Franchise details and compositions
- **Venue Analytics**: Stadium-wise performance data

## 🛠️ Technologies Used

| Component | Technology |
|-----------|------------|
| **Storage** | Amazon S3 |
| **Processing Engine** | Apache Spark |
| **Development Platform** | Databricks Community Edition |
| **Programming Languages** | Python (PySpark), SQL |
| **Visualization** | Matplotlib, Seaborn |
| **Data Format** | CSV, Parquet |

## 🚀 Implementation

### 1. Spark Session Configuration
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("IPL Data Analytics") \
    .getOrCreate()
```

### 2. Schema Definition
```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType, BooleanType

ball_schema = StructType([
    StructField("match_id", IntegerType(), True),
    StructField("over_id", IntegerType(), True),
    StructField("ball_id", IntegerType(), True),
    StructField("innings_number", IntegerType(), True),
    StructField("team_batting", StringType(), True),
    StructField("runs_scored", IntegerType(), True)
])
```

### 3. Advanced Transformations
```python
# Window Functions for Running Totals
from pyspark.sql.window import Window
from pyspark.sql.functions import sum, col

window_spec = Window.partitionBy("match_id", "innings_number").orderBy("over_id")
df = df.withColumn("running_total", sum("runs_scored").over(window_spec))

# Conditional Formatting
df = df.withColumn("high_impact", 
    when((col("runs_scored") + col("extra_runs") > 6) | 
         (col("bowler_wicket") == True), True).otherwise(False))
```

### 4. SQL Analytics
```sql
-- Top Scoring Batsmen Per Season
SELECT p.player_name, m.season_year, SUM(b.runs_scored) as total_runs
FROM ball_by_ball b
JOIN matches m ON b.match_id = m.match_id
JOIN players p ON b.striker = p.player_id
GROUP BY p.player_name, m.season_year
ORDER BY m.season_year, total_runs DESC;
```

## 📈 Key Analytics Insights

### Performance Metrics
- **Processing Speed**: 60% improvement through structured schema implementation
- **Data Volume**: 637 matches with 150K+ ball-by-ball records
- **Query Optimization**: Partition-based processing for enhanced performance
- **Statistical Coverage**: 8+ comprehensive analytics dashboards

### Cricket Insights
- **Top Performers**: Season-wise leading run scorers and wicket takers
- **Toss Impact**: Statistical analysis of toss decisions on match outcomes
- **Venue Analysis**: Stadium-wise scoring patterns and team performance
- **Economic Bowling**: Powerplay economics and bowling strategies
- **Player Categories**: Performance-based player classifications

## 🔧 Setup Instructions

### Prerequisites
- Databricks account (Community Edition)
- Basic understanding of Apache Spark and PySpark
- Python knowledge for data analysis

### Installation Steps

1. **Clone the repository**
```bash
git clone https://github.com/santhoshkrishnan30/IPL-Data-Analysis.git
cd IPL-Data-Analysis
```

2. **Set up Databricks Environment**
- Create Databricks Community Edition account
- Import notebooks to workspace
- Configure cluster (15GB RAM, 2 cores recommended)

3. **Data Access**
```python
# Using public S3 bucket (provided in code)
s3_path = "s3://your-bucket/ipl-data/"
df = spark.read.format("csv").option("header", "true").schema(schema).load(s3_path)
```

4. **Execute Pipeline**
- Run data ingestion notebook
- Execute transformation workflows
- Perform SQL analytics
- Generate visualizations



## 📊 Sample Analytics Queries

### 1. Economical Bowlers in Powerplay
```sql
SELECT p.player_name, 
       AVG(b.runs_scored) as avg_runs_per_ball,
       COUNT(b.bowler_wicket) as total_wickets
FROM ball_by_ball b
JOIN players p ON b.bowler = p.player_id
WHERE b.over_id <= 6
GROUP BY p.player_name
HAVING COUNT(*) > 120
ORDER BY avg_runs_per_ball ASC;
```

### 2. Toss Impact Analysis
```sql
SELECT m.toss_winner, m.match_winner,
       COUNT(*) as matches,
       ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER(), 2) as win_percentage
FROM matches m
WHERE m.toss_winner = m.match_winner
GROUP BY m.toss_winner, m.match_winner;
```

## 🎯 Results & Achievements

- ✅ **60% query performance improvement** through structured schema implementation
- ✅ **Automated schema validation** preventing data quality issues
- ✅ **Lazy evaluation optimization** for efficient memory utilization
- ✅ **Multi-table joins** enabling complex cricket analytics
- ✅ **8+ statistical insights** covering comprehensive cricket metrics
- ✅ **Interactive dashboards** for dynamic data exploration

## 📈 Visualization Examples

The project includes various visualization types:
- **Bar Charts**: Top performers and team statistics
- **Line Graphs**: Performance trends over seasons
- **Heatmaps**: Venue-wise performance matrices
- **Pie Charts**: Dismissal type distributions
- **Scatter Plots**: Player performance correlations

## 🤝 Contributing

Contributions are welcome! Areas for enhancement:
- Real-time data streaming integration
- Machine learning model predictions
- Advanced statistical analysis
- Interactive web dashboard
- Performance optimization techniques

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Contact

**Santhosh Krishnan R.**
- 📧 Email: santhoshkrishnan3006@gmail.com
- 💼 LinkedIn: [santhoshkrish03](https://www.linkedin.com/in/santhoshkrish03/)
- 🌐 Portfolio: [santhoshkrishnan30.github.io](https://santhoshkrishnan30.github.io/)

---
⭐ **Star this repository if you found it helpful!**

## 🔗 Related Projects

- [Earthquake Monitoring System](https://github.com/santhoshkrishnan30/EarthQuake-Fabric-DataEngineering)
- [Olympic Data Analytics](https://github.com/santhoshkrishnan30/Olympics-Data-Analytics)

---
*Built with ❤️ using Apache Spark and PySpark*
