
Sample Web Application Deployment with Docker:

This project demonstrates the deployment of a sample web application using Docker containers. The application is built from scratch, avoiding the use of pre-existing Docker container images.


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

Steps:
1. Clone this repository to your local machine.
2. Navigate to the project directory.
3. Build and start the Docker containers using Docker Compose

```bash
  docker-compose up --build

```

Access the web application by navigating to http://localhost in your web browser.






## Project Directory Structure
```bash
  .
├── docker-compose.yml
├── Dockerfile
├── index.php
└── README.md

```
## Files in This Repository
Dockerfile

Defines the web server environment:

Dockerfile
```bash
FROM php:7.4-apache

WORKDIR /var/www/html

COPY . /var/www/html/

RUN docker-php-ext-install mysqli

EXPOSE 80

```
docker-compose.yml

Defines the services and their configurations:
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
index.php

The PHP script for handling form submissions and database interactions:

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
## Author

G23AI2124- g23ai2124@iitj.ac.in

Chinmayee BM- https://github.com/Chinmayee-Codes

