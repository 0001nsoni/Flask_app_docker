# Flask App with MySQL Docker Setup
![Flask App UI](images/flask-ui.png)

This is a simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## Prerequisites
Before you begin, make sure you have the following installed:

- **Docker**
- **Git** (optional, for cloning the repository)

## Setup
### Clone the repository (if you haven't already):
```sh
git clone https://github.com/your-username/your-repo-name.git
```

### Navigate to the project directory:
```sh
cd your-repo-name
```

### Create a `.env` file in the project directory to store your MySQL environment variables:
```sh
touch .env
```

### Open the `.env` file and add your MySQL configuration:
```ini
MYSQL_HOST=mysql
MYSQL_USER=your_username
MYSQL_PASSWORD=your_password
MYSQL_DB=your_database
```

## Usage
### Start the containers using Docker Compose:
```sh
docker-compose up --build
```

### Access the Flask app in your web browser:
- **Frontend:** [http://localhost](http://localhost)
- **Backend:** [http://localhost:5000](http://localhost:5000)

### Create the `messages` table in your MySQL database:
Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL command:
```sql
CREATE TABLE messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    message TEXT
);
```

### Interact with the app:
- Visit [http://localhost](http://localhost) to see the frontend. You can submit new messages using the form.
- Visit [http://localhost:5000/insert_sql](http://localhost:5000/insert_sql) to insert a message directly into the `messages` table via an SQL query.

## Cleaning Up
To stop and remove the Docker containers, press `Ctrl+C` in the terminal where the containers are running, or use the following command:
```sh
docker-compose down
```

## Running Without Docker Compose
### Build a Docker image from the Dockerfile:
```sh
docker build -t flaskapp .
```

### Create a Docker network:
```sh
docker network create twotier
```

### Run MySQL container:
```sh
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=twotier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7
```

### Run Flask backend container:
```sh
docker run -d \
    --name flaskapp \
    --network=twotier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest
```

## Notes
- Make sure to replace placeholders (e.g., `your_username`, `your_password`, `your_database`) with your actual MySQL configuration.
- This is a basic setup for demonstration purposes. In a production environment, follow best practices for security and performance.
- Be cautious when executing SQL queries directly. Validate and sanitize user inputs to prevent vulnerabilities like SQL injection.
- If you encounter issues, check Docker logs and error messages for troubleshooting:
  ```sh
  docker logs flaskapp
  docker logs mysql
  ```

