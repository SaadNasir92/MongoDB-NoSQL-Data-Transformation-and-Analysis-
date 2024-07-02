# NoSQL Data Transformation and Analysis: UK Food Standards Agency Restaurant Ratings

## Project Overview

The UK Food Standards Agency evaluates various establishments across the United Kingdom, providing them with food hygiene ratings. I have been contracted by the editors of a food magazine, *Eat Safe, Love*, to evaluate this ratings data to assist journalists and food critics in deciding where to focus future articles. This project involved transforming and analyzing a dataset of approximately 38,780 restaurants.

## Objectives

- Import the provided JSON dataset into MongoDB.
- Perform necessary data transformations and additions to prepare the data for analysis.
- Execute advanced NoSQL queries using aggregation pipelines and datatype conversions.
- Visualize the results of NoSQL queries using pandas in the form of tables.

Note: Some prompts were made up for the purpose of displaying ability to perform basic CRUD operations in NoSQL.

## Tools and Technologies

- **MongoDB**: Database Solution used.
- **Pandas**: For visualizing the NoSQL query results in table form.
- **PyMongo**: For integration within Python & to write my queries.
- **MongoDB Shell**: For dumping the JSON file & writing ad-hoc queries.
- **Python**: For scripting and automating data transformations and analysis.

## Try this at home!

Make sure the BASH instance is in the same location as the JSON file.
```
Import the data provided in the `establishments.json` file from your Terminal. Name the database `uk_food` and the collection `establishments`.

Within this markdown cell, copy the line of text below into the terminal (in the same directory as the json file you want to import).

e.g.: Import the dataset with `mongoimport --type json -d uk_food -c establishments --drop --jsonArray establishments.json` from a Bash terminal within the resources folder.
```
```bash
`mongoimport --type json -d uk_food -c establishments --drop --jsonArray establishments.json`
```

## Data Transformation and Preparation

The dataset required several transformations and additions before it could be analyzed effectively. Key steps included:

1. **Data Import**: Imported the JSON dataset into MongoDB using the MongoDB Shell.
2. **Data Cleaning**: Identified and handled missing or inconsistent data using PyMongo & Python.
3. **Data Transformation**: Converted data types and standardized formats as needed.
4. **Data Enrichment**: Added additional fields or computed new metrics to enhance the dataset.

## NoSQL Queries and Analysis

### Aggregation Pipelines

Used advanced aggregation pipelines to:

- Group data by various dimensions (e.g., region, type of establishment).
- Calculate statistics such as average hygiene rating, distribution of ratings, etc.
- Filter and sort establishments based on specific criteria.

For Eg:
```python
# Pipeline:
match_q = {
    '$match': {'scores.Hygiene': 0}
}
group_q = {
    '$group': {'_id': '$LocalAuthorityName', 'count':{'$sum':1}}
}
sort_q = {
    '$sort': {'count': -1}
}

pipeline = [match_q, group_q, sort_q]

pipe_results = list(establishments.aggregate(pipeline))
print(len(pipe_results))

for x in range(10):
    print(pipe_results[x])
```

### Data Type Conversions

Performed data type conversions to:

- Ensure numeric fields were correctly typed for calculations.
- Convert date fields to appropriate formats for temporal analysis.

```python
# Set non 1-5 Rating Values to Null
non_ratings = ["AwaitingInspection", "Awaiting Inspection", "AwaitingPublication", "Pass", "Exempt"]
establishments.update_many({"RatingValue": {"$in": non_ratings}}, [ {'$set':{ "RatingValue" : None}}])

# Check initial datatypes
rating_ints = establishments.count_documents({'RatingValue':{'$type': ['int']}})
rating_nulls = establishments.count_documents({'RatingValue': {'$type': ['null']}})
rating_strs = establishments.count_documents({'RatingValue': {'$type': ['string']}})
total_records = establishments.count_documents({})

total_records, rating_ints, rating_nulls, rating_strs

# Change the data type from String to Integer for RatingValue
establishments.update_many({}, [{"$set": {'RatingValue': {"$toInt":"$RatingValue"}}}])

# Check fixed datatypes
rating_ints = establishments.count_documents({'RatingValue':{'$type': ['int']}})
rating_nulls = establishments.count_documents({'RatingValue': {'$type': ['null']}})
rating_strs = establishments.count_documents({'RatingValue': {'$type': ['string']}})
total_records = establishments.count_documents({})

total_records, rating_ints, rating_nulls, rating_strs
```
## Visualization with Pandas

Visualized the results of NoSQL queries using pandas:

- Created tables summarizing key findings.
- Used pandas' rich functionality to manipulate and display the data effectively.

## Key Findings

- **Regional Analysis**: Identified regions with the highest and lowest average hygiene ratings.
- **Type of Establishment**: Compared hygiene ratings across different types of establishments (e.g., restaurants, cafes, takeaways).
- **Rating Distribution**: Analyzed the distribution of hygiene ratings to identify trends and outliers.

## Additional Insights

This project demonstrates my ability to:

- Work with NoSQL databases and perform complex data transformations.
- Write and optimize advanced NoSQL queries, including aggregation pipelines.
- Integrate NoSQL data with pandas for enhanced data analysis and visualization.
- Derive actionable insights from large datasets to support decision-making.

## Conclusion

This project provided valuable insights into the food hygiene ratings of establishments across the UK, helping *Eat Safe, Love* focus their future articles. It also showcases my proficiency in NoSQL data transformation and analysis, which I believe would be beneficial for employers looking for expertise in managing and analyzing unstructured data.

## Notebooks

- [Database and Jupyter Notebook Set Up](NoSQL_setup.ipynb)
- [Restaurant Ratings Analysis](rest_analysis.ipynb)
