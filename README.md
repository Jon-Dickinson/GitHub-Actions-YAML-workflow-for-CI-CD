# AWS

```
.github/workflows/ci-cd.yml
```

```
name: CI/CD Pipeline

# Trigger the workflow on push or pull request to the main branch
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  # Job for building and testing the application
  build-and-test:
    runs-on: ubuntu-latest

    steps:
    # Checkout the code from the repository
    - name: Checkout code
      uses: actions/checkout@v2

    # Set up Node.js environment
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'  # Specify the Node.js version to use

    # Install dependencies
    - name: Install dependencies
      run: npm install

    # Run tests (optional, but recommended)
    - name: Run tests
      run: npm test

  # Job for deploying the app
  deploy:
    needs: build-and-test  # This job depends on the successful completion of the build job
    runs-on: ubuntu-latest

    steps:
    # Checkout the code again in the deployment job
    - name: Checkout code
      uses: actions/checkout@v2

    # Set up Node.js again for deployment
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    # Install dependencies
    - name: Install dependencies
      run: npm install

    # Build the app
    - name: Build the app
      run: npm run build

    # Configure AWS CLI (for deployment to AWS S3)
    - name: Set up AWS CLI
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: 'us-east-1'  # Set the AWS region for your bucket

    # Deploy to AWS S3
    - name: Deploy to S3
      run: |
        aws s3 sync build/ s3://your-bucket-name --delete

```


# Azure


```
.github/workflows/ci-cd.yml
```

```
name: CI/CD Pipeline

# Trigger the workflow on push or pull request to the main branch
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  # Job for building and testing the application
  build-and-test:
    runs-on: ubuntu-latest

    steps:
    # Checkout the code from the repository
    - name: Checkout code
      uses: actions/checkout@v2

    # Set up Node.js environment
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'  # Specify the Node.js version to use

    # Install dependencies
    - name: Install dependencies
      run: npm install

    # Run tests (optional, but recommended)
    - name: Run tests
      run: npm test

  # Job for deploying the app
  deploy:
    needs: build-and-test  # This job depends on the successful completion of the build job
    runs-on: ubuntu-latest

    steps:
    # Checkout the code again in the deployment job
    - name: Checkout code
      uses: actions/checkout@v2

    # Set up Node.js again for deployment
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    # Install dependencies
    - name: Install dependencies
      run: npm install

    # Build the app
    - name: Build the app
      run: npm run build

    # Deploy to Azure App Service
    - name: Deploy to Azure Web App
      uses: azure/webapps-deploy@v2
      with:
        app-name: 'your-app-name'
        slot-name: 'production'
        publish-profile: ${{ secrets.AZURE_PUBLISH_PROFILE }}  # Store securely in GitHub secrets
        package: '.' # Package the contents of the repository

```
