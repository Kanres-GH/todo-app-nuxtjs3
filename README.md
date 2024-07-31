# To-Do List Manager

## Description

The To-Do List Manager is a single-page application built with Nuxt3 and Tailwind CSS. It allows users to manage their to-do list, including adding, editing, and deleting tasks. The application uses localStorage to persist data, ensuring that tasks remain saved even after the page is reloaded.

## Features

- Add new tasks
- Edit existing tasks
- Delete tasks
- Persist tasks using localStorage

## Installation

### Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/) (version 14.x or higher)
- [npm](https://www.npmjs.com/) (version 6.x or higher)
- [Docker](https://www.docker.com/) (optional, for containerization)

### Steps

1. Clone the repository:

   ```bash
   git clone <repository-url>
   cd todo-app
   ```
2. Install dependencies:
    ```bash
    npm install
    ```
## Launch

### Development Server

1. Run the development server:
    ```bash
    npm run dev
    ```
2. Open your browser and navigate to:
    ```bash
    http://localhost:3000
    ```
## Deployment

The To-Do List Manager can be deployed using Docker. Follow the steps below to containerize and run the application.

### Docker Deployment
1. Build the Docker image:
    ```bash
    docker build -t todo-app .
    ```
2. Run the Docker container:
    ```bash
    docker run -p 3000:3000 todo-app
    ```
3. Open your browser and navigate to:
    ```bash
    http://localhost:3000
    ```
## Makefile
A Makefile is provided to simplify the common commands for building and running the application with Docker.
### Available Commands
- make build: Build the Docker image for the To-Do List Manager.
- make run: Run the Docker container for the To-Do List Manager.
- make stop: Stop the running Docker container.

## Input Validation
The application includes input validation to ensure that tasks cannot be added without a valid name. An error message will be displayed if the input is empty or if the input contains invalid characters.
