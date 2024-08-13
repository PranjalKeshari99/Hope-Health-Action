# Moved from Gitlab to Github since gitlab server got closed #

# CBR Manager #

The CBR (Community Based Rehabilitation) Manager is a web application designed to enhance rehabilitation efforts within refugee communities in BidiBidi and Paloryina, Northern Uganda. The tool supports the work of Hope Health Action (HHA) by streamlining data management and interaction between different user roles: administrators, who oversee operations, and workers, who engage directly with clients. Workers are tasked with inputting client data and completing visit forms. Although the application is currently web-based, a future mobile version is anticipated. Presently, workers generally access the application on mobile devices, while administrators use desktop computers. The application aims to include features for generating data reports. The stack includes React for the frontend, NodeJS for the backend, and PostgreSQL as the database, with Sequelizer employed to simplify database queries.

## Directory Structure ##

- Root Directory: Hosts the NodeJS backend server files and directories.
    - client directory: Contains the frontend React application.
    - Key backend directories/files:
        - app.js: Entry point for the NodeJS server.
        - config: Database connection configurations.
        - migrations: Sequelizer migration files used to create and manage database tables.
        - models: Defines data models used by the application.
        - routes: Handles backend API requests for data operations.
        - seeders: Preloads sample data for testing and demos.
        - tests/server: Backend server test cases.
- Client Directory:
    - app.js: Main file for the React application.
    - routes.js: Defines web application routes and their corresponding components.
    - components: Reusable functional components.
    - css: Stylesheets for various pages.
    - pages: Functional components organized into folders, each representing a specific route or set of related pages.

## Build Directions / Dependencies / Run Instructions ##

### Installing Dependencies ###

Run in project root directory:
1) "npm install" for backend dependencies
2) "npm run client-install" for client dependencies

### Setting up the Database ###

1) Download any local [PostgreSQL server](https://www.postgresql.org/download/)

2) Connect to the server and create an empty database using the terminal or any GUI tool (TablePlus is a good option for Mac users).

3) Inside the project root directory create a ".env" file to setup your environment variables, the ".env" file should include:
    - DB_USERNAME = "your_postgres_username"
    - DB_PASSWORD = your_postgres_password (set it to null if doesn't exist)
    - DB_HOST = "localhost"
    - DB_DATABASE = "database_name"
    - DB_PORT = your_postgres_port (by default it is 5432)
    - DB_URL = "username:password@host_name:port/db_name" (in the same format) 

4) Run "node app.js" to start the server, if you get a "DB connection established successfully" message then you have successfully connected to your database, otherwise an error message will be shown in the console.

5) Run "npx sequelize-cli db:migrate" to let Sequelize create the database tables for you.

6) (Optional) Run "npx sequelize-cli db:seed --seed /seeders/demo3" to load the tables with the provided stock data.

### Enabling Google Maps Feature ### 

Some pages will display a Google Maps with a marker, which requires a Google Maps API key. To set this up for localhost:

1) Follow this [guide](https://developers.google.com/maps/documentation/embed/get-api-key) to obtain a Google Maps API key.

2) Create a .env file within the /client folder with the following "REACT_APP_GOOGLE_MAPS_API_KEY='api key'".

3) Enable the JavaScript Google Maps API in the Google Cloud Platform console.

**Note**: The above steps are optional for running on localhost, but will display "Map cannot load properly" error along with the "For developer purposes only" watermark. However, the map will still display the correct location with the marker in the right place.

### Additional Environmental Settings ###

For the web application to run on localhost, you'll need to add the following additional environmental variables to the root directory .env file:
- ACCESS_TOKEN_SECRET=474e765be37690bee00c75c3a3e5c8e6a2c8608f0798b1b6364e4d04d13edc324f4b5c94529e4841bd59cfd67160b1479a8acc1d9183bc2a05ff5041b1363e72

### Run Instructions ###

After all the set up is done, use the command "npm run start" in the project root directory and the client and server will start simultaneously.

## Deploying to Heroku ##

To deploy to Heroku, you'll need to do the following steps (create an Heroku account first if you didn't already do so):

1. Create an application on Heroku either through the web interface for the terminal commands.
2. Follow the provided instructions for deployment on the page after creating the application (i.e. git init -> heroku git:remote -a <app_name>)
3. Run "heroku addons:create heroku-postgresql:hobby-dev".
4. Push your code to the master branch of Heroku, which should trigger a build.
5. After the application has been successfully built by Heroku, run "heroku run npx sequelize-cli db:migrate" to create the necessary tables.
6. Before the application is fully functional, you'll need to add the environment variables ACCESS_TOKEN_SECRET (.env) and REACT_APP_GOOGLE_MAPS_API_KEY (client/.env). Follow this [guide](https://devcenter.heroku.com/articles/config-vars) for this process.
7. (Optional) Run "heroku run npx sequelize-cli db:seed --seed /seeders/demo3" to load web application with provided stock data.

## Testing ##

Backend testing is set up and can be run after setting additional environment variables for the test database:

    - Add DB_DATABASE_TEST and DB_URL_TEST to the .env file.
    - Prepare the test environment with npm run pretest.
    - Run tests using npm run test:server.
    - Note: Only a few API endpoints currently have tests implemented.
