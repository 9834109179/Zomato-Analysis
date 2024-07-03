<p>

# Zomato Dataset Analysis

This project focuses on analyzing the Zomato dataset to gain insights into the restaurant landscape, customer preferences, and other trends. The primary steps include data cleaning, preprocessing, and exploratory data analysis (EDA).

---

## Dataset Overview

The Zomato dataset comprises various attributes related to restaurants, including:

- **name**: Name of the restaurant
- **rate**: Average rating of the restaurant
- **votes**: Number of votes received
- **location**: Location of the restaurant
- **cuisines**: Types of cuisines offered
- **rest_type**: Type of restaurant
- **online_order**: Whether the restaurant accepts online orders
- **book_table**: Whether the restaurant accepts table bookings
- **approx_cost(for two people)**: Approximate cost for two people

## Data Cleaning and Preprocessing

Several steps were taken to clean and preprocess the data:

1. **Handling Missing Values**: Identified columns with missing values and calculated the percentage of missing data for each column.
   ```python
   feature_na = [i for i in data.columns if data[i].isnull().sum() > 0]
   for i in feature_na:
       print(f"{i} has {np.round((data[i].isnull().sum()/len(data[i])*100), 4)}% null values")
   ```

2. **Cleaning the `rate` Column**: Removed rows with missing ratings, split and cleaned the `rate` column, and converted it to a numeric type.
   ```python
   data.dropna(subset=['rate'], axis=0, inplace=True)
   data['rate'] = data['rate'].apply(lambda x: x.split('/')[0].strip())
   data['rate'].replace(['NEW', '-'], 0, inplace=True)
   data['rate'] = data['rate'].astype(float)
   ```

3. **Handling Other Columns**: Dropped rows with missing values in the `approx_cost(for two people)` column.
   ```python
   data.dropna(axis=0, subset=['approx_cost(for two people)'], inplace=True)
   ```

## Exploratory Data Analysis (EDA)

The cleaned data was analyzed to derive meaningful insights:

1. **Average Rating for Each Restaurant**: Calculated and visualized the average rating for each restaurant.
   ```python
   rating = pd.pivot_table(data, index='name', values='rate').sort_values(['rate'], ascending=False)
   plt.figure(figsize=(15,8))
   sns.barplot(x=rating[0:20].rate, y=rating[0:20].index, orient="h")
   plt.show()
   ```

2. **Distribution of Ratings**: Visualized the distribution of ratings.
   ```python
   sns.set_style('whitegrid')
   sns.distplot(data['rate'])
   plt.show()
   ```

3. **Normality Test**: Tested whether the rating distribution is normal.
   ```python
   from scipy.stats import normaltest
   stat, p = normaltest(data['rate'])
   if p > 0.05:
       print("Normal distribution")
   else:
       print("Not a normal distribution")
   ```

4. **Top Restaurant Chains**: Identified and visualized the top restaurant chains.
   ```python
   chains = data['name'].value_counts()[0:15]
   plt.figure(figsize=(10,7), dpi=110)
   sns.barplot(x=chains, y=chains.index, palette='deep')
   plt.xlabel("Number of Restaurants")
   plt.show()
   ```

5. **Online Order Acceptance**: Analyzed how many restaurants accept online orders.
   ```python
   x = data.online_order.value_counts()
   labels = ['accepted', 'not-accepted']
   plt.pie(x, labels=labels, explode=[0.0, 0.1], autopct='%1.1f%%')
   plt.show()
   ```

6. **Table Bookings**: Analyzed how many restaurants accept table bookings.
   ```python
   x = data.book_table.value_counts()
   labels = ['accepted', 'not-accepted']
   plt.pie(x, labels=labels, explode=[0.0, 0.1], autopct='%1.1f%%')
   plt.show()
   ```

7. **Restaurant Types**: Analyzed and visualized the types of restaurants.
   ```python
   rest_type = data.rest_type.value_counts()[0:15]
   plt.figure(figsize=(20,12))
   plt.bar(rest_type.index, rest_type)
   plt.show()
   ```

8. **Top Cuisines**: Identified and visualized the top 10 cuisines.
   ```python
   cuisines = data.cuisines.value_counts()[0:10]
   sns.barplot(x=cuisines.index, y=cuisines)
   plt.xticks(rotation=75)
   plt.show()
   ```

9. **Cost Distribution**: Analyzed the distribution of the cost for two people.
   ```python
   sns.distplot(data['approx_cost(for two people)'])
   plt.show()
   ```

## Conclusion

This project provides a detailed analysis of the Zomato dataset, offering insights into customer preferences, restaurant types, and other trends. These insights can help restaurant owners and stakeholders make data-driven decisions to improve their services and marketing strategies.

## Requirements

- Python 3.x
- pandas
- numpy
- matplotlib
- seaborn
- scipy

## Usage

To run this analysis, clone the repository and execute the Jupyter notebook or Python script provided. Make sure to install the required libraries using:
```bash
pip install -r requirements.txt
```

## License

This project is licensed under the MIT License.

---
  
</p>
