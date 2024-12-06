
# **Vintage Analysis Tool**

Created by [Granum Technologies LLC](https://www.granum-tech.com/)  

<a href="https://colab.research.google.com/github/granum-tech/open_finance_library/blob/main/vintage_analysis/src/vintage_analysis.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

<p align="center">
<table>
  <tr>
    <td>
      <img src="https://github.com/granum-tech/open_finance_library/blob/main/vintage_analysis/examples/output/sum/quarterly/vintage_analysis_quarterly_sum.png?raw=true" width="300" alt="Quarterly Percent"/>
    </td>
    <td>
      <img src="https://github.com/granum-tech/open_finance_library/blob/main/vintage_analysis/examples/other/matrix_example_02.jpg?raw=true" width="700" alt="Quarterly Percent"/>
    </td>
  </tr>
</table>
</p>

## **Contents**
- [Introduction](#introduction)
  - [Steps to Use the Tool](#steps-to-use-the-tool)
- [Variables](#variables)
- [Input Requirements](#input-requirements)
  - [Example File](#example-file)
  - [Field Explanation](#field-explanation)
- [Function Descriptions](#function-descriptions)
  - [1. `load_data`](#load-data)
  - [2. `validate_data`](#validate-data)
  - [3. `calculate_vintage_matrix`](#calculate-vintage-matrix)
  - [4. `plot_vintage_matrix`](#plot-vintage-matrix)

<a name="introduction"></a>
## **Introduction**

This tool helps you analyze loan performance over time by categorizing loans into "vintages" based on their boarding dates. Each vintage tracks how loans perform over time, measuring metrics like cumulative charge-offs as a percentage of the original amount financed. This is commonly used in finance to evaluate risk and loan quality.

For further details, an in-depth guide, and example input and output files please visit our official [GitHub Repository](https://github.com/granum-tech/open_finance_library/tree/main/vintage_analysis).

<a name="steps-to-use-the-tool"></a>

## **Steps to Use the Tool**

1. **Change the Variables**  
   Update the variables at the top of the notebook to match your preferences. For example:
   ```python
   period_type = 'yearly'
   calculation_type = 'percent'
   output_file_path = None
2. **Upload Your File**  
   Run the notebook, and it will prompt you to upload your file. You can upload either a CSV or XLSX file. The code will automatically rename your file to input.csv or input.xlsx. *Note that if you run all cells the code will wait for you to upload the file until proceeding*.  
   Example input files can be found in the [GitHub Repository](https://github.com/granum-tech/open_finance_library/tree/main/vintage_analysis/examples/input).
3. **Run All**  
   Click "Runtime" then "Run all" or use the hotkey "Ctrl+F9" to execute all cells in the notebook once variables have been set. Ensure you upload your file and then note outputs including possible data quality errors. Your output matrix .xlsx file and graph .png file will be in the default directory.
4. **Output**  
   - **Excel File:** A detailed analysis saved to the specified file path or defaulting i.e. `vintage_analysis_quarterly_percent.xlsxs`
   - **Graph:** A PNG image visualizing the results i.e. `vintage_analysis_quarterly_percent.png`  

   Example outputs can be found in the [GitHub Repository](https://github.com/granum-tech/open_finance_library/tree/main/vintage_analysis/examples/output).
5. **Diagnostics**  
   After the last code block at the bottom of the notebook you will see outputs of the functions including `validate_data` which will print any errors that it is looking for. This is a good way to troubleshoot. You can also review the [GitHub Repository](https://github.com/granum-tech/open_finance_library/tree/main/vintage_analysis) or reach out to the author at info@granum-tech.com.

<a name="variables"></a>
## **Variables**

Below you can set the following variables:

- **`period_type`**: The time period for analysis:
  - `'monthly'`: Group loans into monthly vintages.
  - `'quarterly'`: Group loans into quarterly vintages.
  - `'yearly'`: Group loans into yearly vintages.
- **`calculation_type`**: The type of calculation for the analysis:
  - `'sum'`: Calculate the cumulative net call-off sum.
  - `'percent'`: Calculate the percentage of cumulative charge-offs relative to the original amount financed.
- **`output_file_path`**: The file path for saving the analysis results. Leave it as `None` to use the default naming.

<a name="input-requirements"></a>
## **Input Requirements**

<a name="example-file"></a>
### **Example File**

Your input file should have the following format:

| loan_id | boarding_date | charge_off_date | net_call_off | original_amount_financed |
|---------|---------------|-----------------|--------------|--------------------------|
| 1       | 12/11/2015    |                 |              | 122072.00               |
| 2       | 2/9/2016      |                 |              | 50595.00                |
| 3       | 2/10/2016     |                 |              | 38905.00                |
| 4       | 2/12/2016     | 9/11/2024       | 40336.00     | 118506.00               |

Alternatively, your file can be in CSV format like below:

```
loan_id,boarding_date,charge_off_date,net_call_off,original_amount_financed  
1,12/11/2015,,,122072  
2,2/9/2016,,,50595  
3,2/10/2016,,,38905
```

<a name="field-explanation"></a>
### **Field Explanation**

- **loan_id**: A unique identifier for each loan (e.g., 1, 2, 3).
- **boarding_date**: The date the loan was issued (format: MM/DD/YYYY).
- **charge_off_date**: The date the loan was written off, if applicable.
- **net_call_off**: The amount charged off for the loan (optional, defaults to 0 if not provided).
- **original_amount_financed**: The total amount financed when the loan was issued.

Ensure that column names and data formats match these requirements.

<a name="function-descriptions"></a>
## **Function Descriptions**

<a name="load-data"></a>
### **`load_data(file_path)`**  
Loads the uploaded file into a pandas DataFrame. It supports both CSV and Excel formats.

<a name="validate-data"></a>
### **`validate_data(df, calculation_type, period_type)`**  
Validates the input data and variables:
- Checks for required columns
- Ensures valid date formats
- Ensures valid `calculation_type` and `period_type` input variable values

<a name="calculate-vintage-matrix"></a>
### **`calculate_vintage_matrix(df, period_type, calculation_type, output_file_path)`**  
Performs the vintage analysis:
- Groups loans into vintages
- Calculates cumulative net call-off or call-off percentage of vintages origination sum
- Saves the results to an Excel file

<a name="plot-vintage-matrix"></a>
### **`plot_vintage_matrix(vintage_matrix, period_type, calculation_type, output_file_path)`**
Generates a line graph showing the cumulative call-offs over time for each vintage and saves it as an image.