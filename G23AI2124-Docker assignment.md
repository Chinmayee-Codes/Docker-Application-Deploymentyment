
Sample Web Application Deployment using Docker Containers:

This project demonstrates the deployment of a simple web application using Docker containers. The application consists of a form for course registration, which stores the data in a MySQL database. The deployment is done using custom Docker images created from scratch, without using existing Docker container images.


Table of Contents:

* Project Overview

* App Functionality

* Getting Started

* Files in This Repository

* Author
## Project Overview

This project involves two main steps:

Creating Docker images from scratch for a web application and a MySQL database.

Deploying the application using Docker Compose.

The web application is a simple PHP-based course registration form that interacts with a MySQL database.
## App Functionality

The application allows users to register for courses by submitting their name and course details through a form. The data is then stored in a MySQL database.

Features:

* Web server running Apache2 with PHP 7.4

* MySQL database for storing registration data

* Docker containers for isolated and reproducible environments
## Deployment

To deploy the application, follow the steps below:

Prerequisites:

* Docker installed on your system

* Docker Compose installed on your system

Step 1: Install Required Packages

First, install the necessary packages for building Docker images:


```bash
  sudo apt install build-essential dkms linux-headers-$(uname -r)


```
Step 2: Create Docker Images



Create a Dockerfile for the PHP and Apache server:

```bash
FROM php:7.4-apache

WORKDIR /var/www/html

COPY . /var/www/html/

RUN docker-php-ext-install mysqli

EXPOSE 80

```
Step 3: Create Docker Compose Configuration

Create a docker-compose.yml file to define the services:


```bash
version: '3.1'

services:

  web:
    build: .
    ports:
      - "80:80"
    depends_on:
      - db

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: myDB
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```
Step 4: Create the Web Application

Create an index.php file for the web application:

```bash
<?php
$servername = "db";
$username = "root";
$password = "example";
$dbname = "myDB";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $name = $_POST['name'];
    $course = $_POST['course'];

    $sql = "INSERT INTO Students (name, course) VALUES ('$name', '$course')";

    if ($conn->query($sql) === TRUE) {
        echo "New record created successfully";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
}
?>

<!DOCTYPE html>
<html>
<body>

<h2>Course Registration Form</h2>
<form method="post">
  Name:<br>
  <input type="text" name="name" required>
  <br>
  Course:<br>
  <input type="text" name="course" required>
  <br><br>
  <input type="submit" value="Submit">
</form>

</body>
</html>
```
Step 5: Build and Run the Containers

Run the following command to build and start the containers:


```bash
docker-compose up --build

```
Step 6: Initialize the Database

Once the containers are up and running, initialize the database by creating the necessary table.
 Execute the following command:

```bash
docker-compose exec db mysql -u root -pexample myDB
```
Inside the MySQL prompt, create the Students table:

```bash
CREATE TABLE Students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    course VARCHAR(255) NOT NULL
);
```
## Conclusion

The sample web application is running and we can access the course registration form in your web browser by navigating to http://localhost.
## Author

G23AI2124- g23ai2124@iitj.ac.in

Chinmayee BM- https://github.com/Chinmayee-Codes

