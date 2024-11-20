# Dimensional Data Modeling - Day 1 Lab Notes

---

## **1. Objectives**
- Apply the concepts of **struct** and **array** in PostgreSQL for effective data modeling.
- Solve temporal challenges by compactly storing historical data while maintaining usability.
- Understand how cumulative tables support efficient historical and analytical queries.

---

## **2. Key Dataset**
### **Main Table: `player_seasons`**
- Represents NBA players across seasons.
- **Player-level attributes:** Name, height, college, country, draft details (constant across seasons).
- **Season-level stats:** Games played, points, rebounds, assists (change every season).
- **Challenge:** Temporal components create duplicate data and impact efficiency in queries.

---

## **3. Lab Tasks**
### **Task 1: Transform Temporal Data with Structs and Arrays**
- Combine season-specific attributes (e.g., points, rebounds) into a **struct**.
- Use an **array** to group these structs, storing all seasonal data for a player in a single row.
- **Example:** 
  - Original data: 10 rows for 10 seasons of one player.
  - Transformed: 1 row with an array of 10 season stats.

### **Task 2: Create Cumulative Tables**
- Build a cumulative table that tracks historical and current data incrementally:
  - **Player attributes:** Stored once to avoid duplication.
  - **Season stats:** Stored in an array, updated each year.
  - **Current season:** Tracks the latest season for the player.
- **Outcome:** Compact storage and easy access to historical data for analysis.

---

## **4. Cumulative Logic**
### **Key Concepts**
1. **Yesterday and Today Approach**:
   - Combine "yesterday's" cumulative table with "today's" new data using **full outer joins**.
   - Carry forward historical data while adding new updates.

2. **Handling Null Values**:
   - Ensure data is preserved even when no new updates are available (e.g., retired players).
   - Avoid adding unnecessary `null` values by carefully designing the update logic.

3. **Adding Metrics**:
   - Compute additional insights, such as "years since last season" or scoring tiers (e.g., star, good, average, bad).

---

## **5. Benefits of Cumulative Tables**
### **Efficiency Gains**
- Eliminates redundant data storage by grouping temporal data (e.g., seasons) into arrays.
- Preserves historical context for each player while avoiding duplicate attributes.

### **Query Optimization**
- **Faster Queries**: No need for complex aggregations or joins.
- **Compact Design**: Reduces the table size while maintaining usability.

### **Analytical Versatility**
- Allows easy computation of historical metrics without additional transformations.
- Supports powerful queries like:
  - Finding most improved players (comparing first and latest seasons).
  - Filtering by player performance tiers or activity status.

---

## **6. Key Outcomes**
### **Solved Challenges**
- **Temporal Data Issues**: Transformed season-specific data into compact arrays.
- **Historical Tracking**: Incrementally built a complete player history table.
- **Compression Benefits**: Optimized for both storage and query performance.

### **Example Insights**
1. **Player-Level Queries**:
   - Example: Michael Jordan's career trajectory includes a multi-year gap but tracks his comeback seamlessly.

2. **Improvement Analysis**:
   - Identified players with the greatest performance improvements between their first and most recent seasons.

3. **Performance Tiers**:
   - Classified players into scoring categories (star, good, etc.), useful for strategic analysis.

---

## **7. Additional Insights**
### **Cumulative Table Advantages**
- **No Group-By Required**: Queries can be extremely fast and scalable.
- **Historical Context**: Retains a complete picture of players over time.
- **Flexible Design**: Supports various metrics and analytical needs.

### **Trade-Offs**
- Sequential updates: New data must be processed incrementally.
- Table growth: Inactive or irrelevant records must be pruned to maintain efficiency.

---

**Additional Resources**:
   - [Cumulative Table Design](https://github.com/EcZachly/cumulative-table-design)  

---