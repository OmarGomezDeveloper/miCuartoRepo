name: Run Tests on Pull Request

on:
pull_request:
branches:
- main  # Adjust this to the branch you want to trigger the workflow on

jobs:
build-and-test:
runs-on: ubuntu-latest

    steps:
      # Step 1: Check out the code
      - name: Checkout code
        uses: actions/checkout@v3

      # Step 2: Set up .NET environment
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0' # Use the version of .NET your project requires

      # Step 3: Restore dependencies
      - name: Restore dependencies
        run: dotnet restore
        working-directory: src

      # Step 4: Build the project
      - name: Build the project
        run: dotnet build --no-restore --configuration Release
        working-directory: src

      # Step 5: Run tests
      - name: Run tests
        run: dotnet test --no-build --configuration Release --logger "trx;LogFileName=test-results.trx"
        working-directory: src