# Writing-Functions-for-Product-Analysis

This repository contains a collection of Python functions designed to streamline the analysis of Net Promoter Score (NPS) survey data. NPS is a widely used metric for measuring customer satisfaction and loyalty. The project addresses the common problem of repetitive code in data analysis by modularizing the code into functions, making it more maintainable, flexible, and interpretable. It's a DataCamp project.


## Project Description

Have you ever started your data analysis and ended up with repetitive code? Repetitive code is a sign that functions are needed. Functions help keep our code flexible, maintainable, and interpretable. Our colleague Brenda, a product analyst, has written a script to pull Net Promoter Score (NPS) survey data from multiple sources to calculate the NPS score. This code works well, but it violates coding best practices, including Don't Repeat Yourself (DRY). Let's take a look at her code and write some functions for Brenda!

## Project Tasks

### Task 1: DRY: Don't repeat yourself

In this task, we address the issue of repetitive code in Brenda's script. We write a function `convert_csv_to_df` that reads an NPS CSV file and converts it into a DataFrame with a column specifying the source.

[Code for Task 1](#task-01-code)

### Task 2: Verifying the files with the "with" keyword

To ensure that the CSV files are valid, we implement a function `check_csv` that checks if a CSV file contains the required columns: `response_date`, `user_id`, and `nps_rating`. We use the `with` keyword to properly handle file opening and closing.

[Code for Task 2](#task-02-code)

### Task 3: Putting it together with nested functions

To consolidate data from multiple sources, we create a function `combine_nps_csvs` that takes a dictionary of CSV filenames and their corresponding source types. It validates the CSV files and combines them into a single DataFrame.

[Code for Task 3](#task-03-code)

### Task 4: Detractors, Passives, and Promoters

We introduce a function `categorize_nps` that categorizes NPS ratings into detractors, passives, promoters, or invalid based on a predefined rating scale.

[Code for Task 4](#task-04-code)

### Task 5: Applying our function to a DataFrame

We modify the `convert_csv_to_df` function to include a new column `nps_group`, which categorizes NPS ratings using the `categorize_nps` function. We apply this function to a DataFrame using the `apply` method.

[Code for Task 5](#task-05-code)

### Task 6: Calculating the Net Promoter Score

We calculate the Net Promoter Score (NPS) by defining a function `calculate_nps` that takes a DataFrame with the `nps_group` column and computes the NPS score.

[Code for Task 6](#task-06-code)

### Task 7: Breaking down NPS by source

To analyze NPS scores by source type, we create a function `calculate_nps_by_source` that groups the DataFrame by source and applies the `calculate_nps` function to each group.

[Code for Task 7](#task-07-code)

### Task 8: Adding docstrings

In this task, we add docstrings to the functions `combine_nps_csvs`, `calculate_nps`, and `calculate_nps_by_source` to provide information about their purpose, arguments, and return values.

[Code for Task 8](#task-08-code)

## Repository Contents

- `datasets` directory containing NPS CSV files.
- `notebook-before.ipynb`: Code before implementing functions.
- `notebook-after.ipynb`: Code after implementing functions.

## Usage

To use these functions, follow the tasks' code provided in the respective sections of this README. Ensure you have the required CSV files in the `datasets` directory.

## Author

Mohamed Farahat
## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

