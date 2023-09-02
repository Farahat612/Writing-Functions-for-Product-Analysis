# Writing-Functions-for-Product-Analysis

This repository contains a collection of Python functions designed to streamline the analysis of Net Promoter Score (NPS) survey data. NPS is a widely used metric for measuring customer satisfaction and loyalty. The project addresses the common problem of repetitive code in data analysis by modularizing the code into functions, making it more maintainable, flexible, and interpretable. It's a DataCamp project.


## Project Description

Have you ever started your data analysis and ended up with repetitive code? Repetitive code is a sign that functions are needed. Functions help keep our code flexible, maintainable, and interpretable. Our colleague Brenda, a product analyst, has written a script to pull Net Promoter Score (NPS) survey data from multiple sources to calculate the NPS score. This code works well, but it violates coding best practices, including Don't Repeat Yourself (DRY). Let's take a look at her code and write some functions for Brenda!

## Project Tasks

### Task 1: DRY: Don't repeat yourself

In this task, we address the issue of repetitive code in Brenda's script. We write a function `convert_csv_to_df` that reads an NPS CSV file and converts it into a DataFrame with a column specifying the source.

[Code for Task 1](#Task-1:-Inspecting-the-Data)

### Task 2: Verifying the files with the "with" keyword

To ensure that the CSV files are valid, we implement a function `check_csv` that checks if a CSV file contains the required columns: `response_date`, `user_id`, and `nps_rating`. We use the `with` keyword to properly handle file opening and closing.

[Code for Task 2](#Task-2:-Verifying-the-files-with-the-"with"-keyword)

### Task 3: Putting it together with nested functions

To consolidate data from multiple sources, we create a function `combine_nps_csvs` that takes a dictionary of CSV filenames and their corresponding source types. It validates the CSV files and combines them into a single DataFrame.

[Code for Task 3](#Task-3:-Putting-it-together-with-nested-functions)

### Task 4: Detractors, Passives, and Promoters

We introduce a function `categorize_nps` that categorizes NPS ratings into detractors, passives, promoters, or invalid based on a predefined rating scale.

[Code for Task 4](#Task-4:-Detractors,-Passives,-and-Promoters)

### Task 5: Applying our function to a DataFrame

We modify the `convert_csv_to_df` function to include a new column `nps_group`, which categorizes NPS ratings using the `categorize_nps` function. We apply this function to a DataFrame using the `apply` method.

[Code for Task 5](#Task-5:-Applying-our-function-to-a-DataFrame)

### Task 6: Calculating the Net Promoter Score

We calculate the Net Promoter Score (NPS) by defining a function `calculate_nps` that takes a DataFrame with the `nps_group` column and computes the NPS score.

[Code for Task 6](#Task-6:-Calculating-the-Net-Promoter-Score)

### Task 7: Breaking down NPS by source

To analyze NPS scores by source type, we create a function `calculate_nps_by_source` that groups the DataFrame by source and applies the `calculate_nps` function to each group.

[Code for Task 7](#Task-7:-Breaking-down-NPS-by-source)

### Task 8: Adding docstrings

In this task, we add docstrings to the functions `combine_nps_csvs`, `calculate_nps`, and `calculate_nps_by_source` to provide information about their purpose, arguments, and return values.

[Code for Task 8](#Task-8:-Adding-docstrings)

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



## Code for tasks

#Task 1: Inspecting the Data

```python
# Import pandas with the usual alias
import pandas as pd

# Write a function that matches the docstring
def convert_csv_to_df(csv_name, source_type):
    """ Converts an NPS CSV into a DataFrame with a column for the source. 

    Args:
        csv_name (str): The name of the NPS CSV file.
        source_type (str): The source of the NPS responses.

    Returns:
        A DataFrame with the CSV data and a column, source.
    """   
    df = pd.read_csv(csv_name)  # Read the CSV file into a DataFrame
    df['source'] = source_type   # Add a 'source' column
    return df

# Test the function on the mobile data 
convert_csv_to_df("datasets/2020Q4_nps_mobile.csv", "mobile")
```

#Task 2: Verifying the files with the "with" keyword

```python
def check_csv(csv_name):
    """ Checks if a CSV file has three columns: response_date, user_id, and nps_rating

    Args:
        csv_name (str): The name of the CSV file.

    Returns:
        Boolean: True if the CSV is valid, False otherwise 
    """
    # Open csv_name as f using with open
    with open(csv_name, "r") as f:
        first_line = f.readline()
        # Return true if the CSV has the three specified columns
        if first_line == "response_date,user_id,nps_rating\n":
            return True
        # Otherwise, return false    
        else:
            return False

# Test the function on a corrupted NPS file
print(check_csv("datasets/corrupted.csv"))
```

#Task 3: Putting it together with nested functions

```python
def combine_nps_csvs(csvs_dict):
    # Define combine as an empty DataFrame
    combined = pd.DataFrame()
    # Iterate over csvs_dict to get the name and source of the CSVs
    for csv_name, source_type in csvs_dict.items():
        # Check if the csv is valid using check_csv()
        if check_csv(csv_name):
            # Convert the CSV using convert_csv_to_df() and assign it to temp
            temp = convert_csv_to_df(csv_name, source_type)
            # Concatenate temp with combined and assign it to combined
            combined = pd.concat([combined, temp])
        # If the file is not valid, print a message with the CSV's name
        else:
            print(f"{csv_name} is not a valid file and will not be added.")
    # Return the combined DataFrame
    return combined

my_files = {
  "datasets/2020Q4_nps_email.csv": "email",
  "datasets/2020Q4_nps_mobile.csv": "mobile",
  "datasets/2020Q4_nps_web.csv": "web",
  "datasets/corrupted.csv": "social_media"
}

# Test the function on the my_files dictionary 
combined_nps = combine_nps_csvs(my_files)
print(combined_nps)
```

#Task 4: Detractors, Passives, and Promoters

```python
def categorize_nps(x):
    """ 
    Takes a NPS rating and outputs whether it is a "promoter", 
    "passive", "detractor", or "invalid" rating. "invalid" is
    returned when the rating is not between 0-10.

    Args:
        x: The NPS rating

    Returns:
        String: the NPS category or "invalid".
    """
    # Write the rest of the function to match the docstring
    if 0 <= x <= 6:
        return "detractor"
    elif 7 <= x <= 8:
        return "passive"
    elif 9 <= x <= 10:
        return "promoter"
    else:
        return "invalid"

# Test the function 
print(categorize_nps(8))
```

#Task 5: Applying our function to a DataFrame

```python
def convert_csv_to_df(csv_name, source_type):    
    """ Convert an NPS CSV into a DataFrame with columns for the source and NPS group.

    Args:
        csv_name (str): The name of the NPS CSV file.
        source_type (str): The source of the NPS responses.

    Returns:
         A DataFrame with the CSV data and columns: source and nps_group.
    """
    df = pd.read_csv(csv_name)
    df['source'] = source_type
    # Define a new column nps_group which applies categorize_nps to nps_rating
    df['nps_group'] = df['nps_rating'].apply(categorize_nps)
    return df

# Test the updated function with mobile data
print(convert_csv_to_df("datasets/2020Q4_nps_mobile.csv", "mobile"))
```

#Task 6: Calculating the Net Promoter Score

```python
def calculate_nps(dataframe):
    # Calculate the NPS score using the nps_group column 
    promoter_count = dataframe[dataframe['nps_group'] == 'promoter'].shape[0]
    detractor_count = dataframe[dataframe['nps_group'] == 'detractor'].shape[0]
    total_count = dataframe.shape[0]
    nps_score = ((promoter_count - detractor_count) / total_count) * 100
    # Return the NPS Score
    return nps_score

my_files = {
  "datasets/2020Q4_nps_email.csv": "email",
  "datasets/2020Q4_nps_web.csv": "web",
  "datasets/2020Q4_nps_mobile.csv": "mobile",
}

# Test the function on the my_files dictionary
q4_nps = combine_nps_csvs(my_files)
print(calculate_nps(q4_nps))
```

#Task 7: Breaking down NPS by source

```python
def calculate_nps_by_source(dataframe):
    # Group the DataFrame by source and apply calculate_nps()
    nps_by_source = dataframe.groupby('source').apply(calculate_nps)
    # Return a Series with the NPS scores broken by source
    return nps_by_source

my_files = {
  "datasets/2020Q4_nps_email.csv": "email",
  "datasets/2020Q4_nps_web.csv": "web",
  "datasets/2020Q4_nps_mobile.csv": "mobile",
}

# Test the function on the my_files dictionary
q4_nps = combine_nps_csvs(my_files)
print(calculate_nps_by_source(q4_nps))
```

#Task 8: Adding docstrings

```python
# Copy and paste your code for the function from Task 3
def combine_nps_csvs(csvs_dict):
    """
    Combine multiple NPS CSV files into a single DataFrame.

    Args:
        csvs_dict (dict): A dictionary containing file names and source types.

    Returns:
        pd.DataFrame: A DataFrame containing combined NPS data from the CSV files.
    """
    combined = pd.DataFrame()
    for csv_name, source_type in csvs_dict.items():
        if check_csv(csv_name):
            temp = convert_csv_to_df(csv_name, source_type)
            combined = pd.concat([combined, temp], ignore_index=True)
        else:
            print(csv_name + " is not a valid file and will not be added.")
    return combined

# Copy and paste your code for the function from Task 6
def calculate_nps(dataframe):
    """
    Calculate the Net Promoter Score (NPS) of a DataFrame.

    Args:
        dataframe (pd.DataFrame): A DataFrame with an 'nps_group' column.

    Returns:
        float: The calculated NPS score.
    """
    promoters = dataframe[dataframe['nps_group'] == 'promoter'].shape[0]
    detractors = dataframe[dataframe['nps_group'] == 'detractor'].shape[0]
    total_responses = dataframe.shape[0]
    nps = ((promoters - detractors) / total_responses) * 100
    return nps

# Copy and paste your code for the function from Task 7
def calculate_nps_by_source(dataframe):
    """
    Calculate NPS scores broken down by source.

    Args:
        dataframe (pd.DataFrame): A DataFrame with NPS data.

    Returns:
        pd.Series: NPS scores broken down by source.
    """
    nps_by_source = dataframe.groupby('source').apply(calculate_nps)
    return nps_by_source
```




