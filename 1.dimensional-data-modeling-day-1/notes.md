# Dimensional Data Modeling Day 1 Notes

---

## **1. Key Concepts**
### **Empathy in Data Modeling**
- **Understand Your Customer**: Who are you delivering data to? Knowing their needs ensures your data structures are usable.
  - **Types of Customers**:
    1. **Data Analysts/Data Scientists**: Need flat, clean datasets (OLAP cubes) with easy-to-query columns for aggregations (e.g., sums, averages).
    2. **Other Data Engineers**: Work with intermediate datasets; complex types (e.g., `struct`, `array`) are acceptable.
    3. **ML Models**: Require flat, numeric, or categorical features; column names matter less.
    4. **End Customers**: Expect user-friendly outputs like charts, visualizations, or metrics, not raw data.

---

## **2. Data Modeling Layers**
### **Layers of Information Refinement**
1. **Production Database Snapshot (OLTP)**:
   - Raw data stored across multiple tables.
   - Optimized for **transactions** (e.g., user details, orders).
   - Requires **joins** to analyze relationships.
2. **Master Data (Intermediate Layer)**:
   - Single table combining raw data.
   - E.g., Airbnb: Combine listings, pricing, availability into a unified table.
   - Easier for downstream pipelines.
3. **OLAP Cube**:
   - Flattened, refined tables for analysis.
   - Aggregated for quick slicing and dicing (e.g., "Revenue by Region by Month").
4. **Metrics**:
   - Highly aggregated, single values or KPIs.
   - E.g., Average listing price = `$150`.

**Example**: Airbnb Data Pipeline  
- **OLTP**: Listings, prices, availability tables.  
- **Master Data**: A single table combining all properties.  
- **OLAP Cube**: Flattened data for analysis by region, host type, etc.  
- **Metrics**: Average occupancy rate.

---

## **3. Cumulative Table Design**
### **What It Is**
- Combines "Yesterday's" and "Today's" data to maintain a historical record.

### **How It Works**
1. **Full Outer Join**:
   - Include data unique to both `yesterday` and `today`.
2. **Coalesce Values**:
   - Merge changes into a single record.
3. **Update Metrics**:
   - Track historical attributes (e.g., days since last activity).

**Example**: User Activity Tracking  
- **Yesterday**: User logs into the app.  
- **Today**: User does not log in.  
- **Output**: User's "Last Active" field updates to 1 day ago.

### **Challenges**
- Sequential backfills only (can't run in parallel).
- Managing PII and pruning stale data (e.g., inactive users).

---

## **4. Complex Data Types**
### **Struct vs. Array vs. Map**
- **Struct**:
  - Acts like a "table within a table."
  - Use when combining related fields (e.g., address with street, city, zip).
- **Array**:
  - Ordered list of values of the same type.
  - Use for repeated values (e.g., list of transactions).
- **Map**:
  - Key-value pairs with flexible keys but uniform value types.
  - Use when the number of attributes varies (e.g., metadata tags).

**Example**: Calendar Data for Airbnb  
- **Struct**: Represents one night's availability (`date`, `price`).  
- **Array**: List of available nights for a listing.  
- **Map**: Availability by custom tags (e.g., `{holiday: high demand}`).

---

## **5. Compression and Optimization**
### **Run-Length Encoding**
- **How It Works**:
  - Compress repeated data by storing the value once and its count.
- **Example**:
  - Instead of: `[A, A, A, B, B]`
  - Store: `[(A, 3), (B, 2)]`
- **When to Use**: Repeated values in sorted columns (e.g., temporal data).

### **Spark Shuffle**
- **What It Does**: Reorganizes data for joins or aggregations, but **breaks sorting**, affecting compression.
- **Solution**:
  - Use complex data types (`array`, `struct`) to preserve structure during operations.

---

## **6. Compactness vs. Usability**
### **Trade-offs**
- **Compact Tables**:
  - Minimize storage.
  - Hard to query; suitable for apps and production systems.
- **Usable Tables**:
  - Flat, simple structure for analysis.
  - Larger size; suitable for OLAP.

### **Middle Ground**
- Use `array`, `struct`, or `map` for semi-compact tables in **master data** layers.

**Example**: Serving App vs. Analyst  
- **App Needs**: Compressed, encoded availability calendar.  
- **Analyst Needs**: Pre-decoded and flattened availability data.

---

**Additional Resources**:
   - [Bootcamp Materials](https://github.com/DataExpert-io/data-engineer-handbook/tree/main/bootcamp/materials/1-dimensional-data-modeling)  
