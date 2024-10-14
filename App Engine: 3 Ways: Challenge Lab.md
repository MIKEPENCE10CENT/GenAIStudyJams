# Task Instructions

## Task 1: Enable the Google App Engine Admin API

1. Go to the **Search Bar** in the Google Cloud Console.
2. Type **"App Engine Admin API"** in the search box.
3. Click on **App Engine Admin API** and then click **Enable**.

---

## Task 2: Download the Hello World App

1. Clone the Hello World app from the repository:

    ```bash
    git clone https://github.com/GoogleCloudPlatform/php-docs-samples.git
    ```

2. Navigate to the Hello World directory:

    ```bash
    cd php-docs-samples/appengine/standard/helloworld
    ```

---

## Task 3: Deploy Your Application

1. Deploy the application to Google App Engine:

    ```bash
    gcloud app deploy
    ```

2. Open the deployed application in your browser:

    ```bash
    gcloud app browse
    ```

---

## Task 4: Deploy Updates to Your Application

1. Edit the `index.php` file:

    ```bash
    nano index.php
    ```

2. Deploy the updated application:

    ```bash
    gcloud app deploy
    ```
