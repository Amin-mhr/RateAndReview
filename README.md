# RateAndReview

This app is designed to manage articles and allow users to rate them. Users can create articles with a title and text, and other users can give ratings (between 1 and 5) to these articles. The app stores the users, articles, and ratings in a database, and for each article, it calculates the average rating and the total number of ratings.

## Getting Started

To get the project running, clone this repository and follow the instructions below.

### Build and Run the Application with Docker Compose

You can build and run the application using Docker Compose:

```bash
docker-compose up -d --build
```
This command will:
1. Build the Docker images for the project.
2. Start the containers in detached mode.

### Access the Application

- **Swagger Documentation**: Visit the Swagger UI at [http://localhost/swagger/](http://localhost/swagger/).
- **Redoc Documentation**: Visit Redoc at [http://localhost/redoc/](http://localhost/redoc/).
- **Admin Panel**: Visit the Django admin panel at [http://localhost/admin/](http://localhost/admin/) (you will need to create a superuser).

### Stopping the Containers

To stop the running containers, use:

```bash
docker-compose down
```
## Fake Rating Detection

To ensure the integrity of article ratings, we process new ratings in periodic intervals and dynamically adjust the average rating. This is achieved by factoring in the statistical differences between the new data and the existing ratings:

1. **Significant Variations**: When the new data has a substantial difference in average rating or standard deviation compared to the previous data, the weight assigned to the new data is reduced. This helps mitigate the impact of anomalous or outlier ratings.

2. **Higher Rating Count**: A greater number of ratings in the new interval increases its influence on the updated average, as more data points carry more statistical significance.

By implementing this method, the app ensures that the average rating reflects a fair and balanced aggregation of user inputs while reducing the potential impact of suspicious or inconsistent rating patterns.

## Scalability and Load Testing

To assess the scalability of the application, I conducted load testing by simulating a large dataset and user activity. This involved creating 1,000,000 users and assigning each user to rate 5 articles, with a total of 20 articles. This setup aimed to replicate a real-world scenario where numerous users interact with the system, helping evaluate its performance under heavy load.

#### Baseline: Naive Solution

To establish a baseline for comparison, I implemented a straightforward but inefficient solution. In this approach, every time an article was displayed, the application executed a query to count the ratings for that article:

- **Naive Query**: The query used was similar to `Rating.objects.filter(article=article).count()`. Each time an article was shown, this query counted the number of ratings for that article.
- **Performance Impact**: While simple to implement, this approach caused significant performance bottlenecks with a large dataset. The database had to repeatedly scan and count rows, which was particularly slow when dealing with millions of ratings, especially if the table lacked proper indexing.

#### Purpose of the Naive Approach

The naive approach served as a benchmark to measure the effectiveness of optimized solutions. By understanding the limitations of the unoptimized implementation, I could compare the baseline performance against improved methods such as caching and database-level optimizations. This helped highlight the tangible benefits of implementing more efficient techniques.

#### Optimized Solution: Caching

To improve scalability and efficiency, I introduced a caching strategy by denormalizing the data. Specifically, I stored the rating count and average rating directly in the `Article` model. These fields were updated only when a new rating was added or an existing rating was modified, rather than recalculating them every time an article was displayed. This significantly reduced the number of queries required for frequently accessed data.

#### Load Testing Results

After populating the database with 100,000 users and 500,000 ratings, I conducted load tests using Locust:

1. **Baseline Testing**: Under a load of 200 active users making requests simultaneously, the naive approach showed significant delays due to repetitive queries.
2. **Optimized Testing**: With the caching solution implemented, the application handled the same load with significantly better performance. Even under a load of 500 active users, the system maintained efficient response times.

#### Conclusion

The optimization efforts, particularly the caching solution, substantially improved the application's scalability and performance. By comparing the results of the naive and optimized approaches, it became clear that reducing repetitive database queries and leveraging denormalized data are effective strategies for handling large datasets and high user activity.

