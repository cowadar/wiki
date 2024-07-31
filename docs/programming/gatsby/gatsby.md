# Gatsby Setup and Usage Guide

This guide provides instructions on setting up and using Gatsby, a powerful tool for building websites. Follow the steps below to get started.

## Setting up Gatsby

1. Run the following command to set up Gatsby:

```shell
$ Gatsby build
```

2. To stop the server, press `Control`.

3. The `Gatsby build` command generates a `public` folder that contains all the optimized assets and files for the project.

## Working with Static Assets

Gatsby converts the project into static files for efficient delivery. However, it is recommended to avoid directly adding assets to the `static` folder to benefit from Gatsby's optimizations. Instead, follow the proper procedure outlined in the documentation.

## Development Server

To start the development server, you have two options:

- Run the command:

  ```shell
  $ Gatsby develop
  ```

- Alternatively, use the npm start script:

  ```shell
  $ npm start
  ```

The server will be accessible at `localhost:8000`, where you can preview your project during development.

## Previewing the Production-Ready Project

To serve the production-ready project locally for preview:

1. Ensure you have previously run `Gatsby build` to generate the `public` folder.

2. Execute the following command:

```shell
$ Gatsby serve
```

You can now access the project at `localhost:9000`.

## Clearing Cache and Troubleshooting

1. To clear the cache and ensure changes take effect, use the following command:

```shell
$ Gatsby clean
```

2. The `Gatsby clean` command deletes the cache and `public` folder. This is particularly useful when working with images and encountering caching issues.

3. If changes to your code are not reflected in the browser automatically, try manually refreshing the page. Although Gatsby uses hot reloading, some scenarios may require a manual refresh.

4. Gatsby version 3 includes an error model that helps identify code errors. If an error occurs, you will see a helpful model pointing out the specific issue.

## Source Directory

Most of the project work should be performed in the `source` directory.

---

Please note that this is a condensed summary of the key points covered in the transcript. For more detailed instructions and explanations, refer to the official Gatsby documentation.