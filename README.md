# MatrixTransform43to19

This repository contains R scripts for transforming matrices with 43 rows into matrices with 19 rows using a normalized concordance matrix. The repository includes scripts for loading, normalizing, transforming, and exporting matrices from Excel files into CSV and Excel formats for further analysis.

# Features

Matrix Transformation: Transforms 43-row matrices into 19-row matrices using a 43x19 concordance matrix.
Normalization: Ensures that the row sums of the concordance matrix equal 1 for accurate scaling.
Input/Output Support: Supports reading matrices from Excel and outputs the transformed matrices into both CSV and Excel formats.
Multiple Matrices: Processes multiple matrices across different Excel sheets.
Requirements

R version 4.0 or higher
Required R libraries: readxl, writexl
Install the necessary packages in R:

r
Copy code
install.packages("readxl")
install.packages("writexl")
Usage

Place your matrices in an Excel file (all_matrices.xlsx), ensuring that the sheets follow the expected naming (e.g., IOT_NW, IOT_NE, IOT_Toscana, etc.).
Place your concordance matrix on a sheet named Concordance.
Run the provided R script to load, normalize, transform, and export the matrices.
Example Script:
r
Copy code
# Load the necessary libraries
library(readxl)
library(writexl)

# Define file paths and load the matrices
file_path <- "all_matrices.xlsx"
concordance_matrix <- read_excel(file_path, sheet = "Concordance", col_names = FALSE)

# Normalize the concordance matrix
normalize_concordance_matrix <- function(concordance) {
    row_sums <- rowSums(as.matrix(concordance))
    sweep(as.matrix(concordance), 1, row_sums, FUN = "/")
}

concordance_matrix_normalized <- normalize_concordance_matrix(concordance_matrix)

# Function to transform a matrix
transform_matrix <- function(matrix_43, concordance) {
    concordance_transpose <- t(as.matrix(concordance))
    concordance_transpose %*% as.matrix(matrix_43) %*% as.matrix(concordance)
}

# Example matrix loading and transformation
iot_nw <- read_excel(file_path, sheet = "IOT_NW", col_names = FALSE)
matrix_19_nw <- transform_matrix(iot_nw, concordance_matrix_normalized)

# Save the transformed matrix
write.csv(matrix_19_nw, "matrix_19_nw.csv", row.names = FALSE)
Output

Transformed matrices are saved as CSV files and optionally into a single Excel file (transformed_matrices.xlsx).
License

This project is licensed under the GNU General Public License v3.0.
