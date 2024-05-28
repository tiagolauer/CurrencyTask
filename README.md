# CurrencyTask

CurrencyTask is a Vue.js application that allows users to track the current and historical prices of various cryptocurrencies.

## Installation

1. Clone the repository to your local environment:

git clone https://github.com/tiagolauer/currency-task.git

2. Navigate to the project directory:

cd currency-task

3. Install project dependencies:

npm install

or

yarn install

## Running the Application

To start the application locally, run the following command:

npm run dev

or

yarn dev

This will launch the development server and open the application in your default web browser.

## Architecture

CurrencyTask utilizes Vue.js for user interface rendering, Vue Router for page navigation, and Axios for making HTTP requests to a third-party API that provides cryptocurrency price data.

## External Libraries

- [Vue.js](https://vuejs.org/): Progressive JavaScript framework for building user interfaces..
- [Vue Router](https://router.vuejs.org/): Official router for Vue.js applications.
- [Axios](https://axios-http.com/): Promise-based HTTP client for the browser and Node.js.

## How to Use the Code

The source code is organized into different directories, each serving a specific purpose:

- `src/components`: Contains Vue components used in the application.
- `src/views`: Contains application views defined by Vue Router.
- `src/App.vue`: Main application component.
- `src/main.ts`: Main entry file that initializes the Vue application.

## Custom Scripts

- `npm run dev`: Starts the development server.
- `npm run build`: Compiles and builds the application for production.
- `npm run preview`: Starts a local server to preview the compiled application.
- `npm run lint`: Runs code linting to ensure compliance with style rules.