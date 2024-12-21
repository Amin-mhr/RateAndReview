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
