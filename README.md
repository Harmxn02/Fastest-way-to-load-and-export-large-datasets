# Fastest way to handle large datasets in Python

## Methodology

- read data
- perform operations on data
- write data

## Libraries

- Pandas
- Pandas with PyArrow backend (parquet)
- Pandas with cuDF
- Polars
- Polars with CUDA (always tested [in a different notebook](./gpu_accelerated/polars.ipynb), because it requires me to use WSL 2)

> Polar's GPU acceleration is not available on Windows, so I have to use WSL 2 to test it, which means the CPU results are together with the GPU results in the same notebook. [Link](./gpu_accelerated/polars.ipynb)
> Polars GPU acceleration requires you to use LazyFrames, so the Polar's comparisons/results will be seperated from the Pandas and PyArrow results

## Dataset

We use a concatenated version of the CICIDS2017 dataset. It is 921 MB in size

![dataset_information](./public/dataset_information.png)

## Conclusion

Using GPU acceleration significantly improves performance when handling large datasets in Python.

cuDF and Polars with CUDA drastically reduce processing times compared to traditional Pandas. PyArrow enhances Pandas' efficiency, but Polars (CPU) remains faster. For optimal performance, leveraging GPU-accelerated libraries like cuDF and Polars with CUDA is recommended.

## Results

### Reading data

We simply read in the dataset

| Library             | Time (seconds)                                                     |
| ------------------- | ------------------------------------------------------------------ |
| Pandas              | 11.3736                                                            |
| Pandas with PyArrow | 1.8045                                                             |
| Polars (CPU)        | 1.1111                                                             |
| Polars with CUDA    | _data reading is not accelerated with this method, so: idem above_ |

Here I compare the speeds of `pandas` and `cuDF` (Pandas with CUDA acceleration), in a seperate notebook. [link](./gpu_accelerated/pandas.ipynb)

| Library                             | Time (seconds) |
| ----------------------------------- | -------------- |
| Pandas                              | 15.4155        |
| cuDF.read_csv                       | 3.0132         |
| Pandas with `%load_ext cudf.pandas` | 2.7375         |

### Performing operations on data

#### Simple operations

We filter the dataset based on the `Destination Port` column, and then we sort it based on the `Destination Port` column

| Library             | Time (seconds) |
| ------------------- | -------------- |
| Pandas              | 0.5041         |
| Pandas with PyArrow | 0.6617         |
| Polars (CPU)        | 0.2960         |

The Polars method is 1.7 times faster than the Pandas method, and 2.2 times faster than the Pandas with PyArrow method

> We are using Polar's LazyFrames here, the results from the table above cannot be compared with the results from the table below

| Library          | Time (seconds) |
| ---------------- | -------------- |
| Polars (CPU)     | 25.0620        |
| Polars with CUDA | 3.3811         |

The CUDA method is 7.4 times faster than the CPU method

#### Slightly more complex operations

[notebook link](./gpu_accelerated/pandas.ipynb)

1. Filter rows (keep values in the `Destination Port` column that are greater than the median)
2. Sort values (sort the `Destination Port` column in ascending order)
3. Group by (group by the `Destination Port` column and count the number of occurrences)
4. Add column (add a column with 2 times the value of the `Destination Port` column)

##### Filter Rows

| Library                             | Time (seconds) |
| ----------------------------------- | -------------- |
| Pandas                              | 0.2585         |
| cuDF                                | 0.0475         |
| Pandas with `%load_ext cudf.pandas` | 0.0313         |

For this operation, cuDF is 5.4 times faster than Pandas, and Pandas with `%load_ext cudf.pandas` is 8.3 times faster than Pandas

##### Sort Values

| Library                             | Time (seconds) |
| ----------------------------------- | -------------- |
| Pandas                              | 1.9335         |
| cuDF                                | 0.0684         |
| Pandas with `%load_ext cudf.pandas` | 0.3990         |

For this operation, cuDF is 28.2 times faster than Pandas, and Pandas with `%load_ext cudf.pandas` is 4.8 times faster than Pandas

##### GroupBy Mean

| Library                             | Time (seconds) |
| ----------------------------------- | -------------- |
| Pandas                              | 3.8155         |
| cuDF                                | 3.4842         |
| Pandas with `%load_ext cudf.pandas` | 3.6203         |

For this operation, cuDF is 1.1 times faster than Pandas, and Pandas with `%load_ext cudf.pandas` is 1.1 times faster than Pandas

##### Add Column

| Library                             | Time (seconds) |
| ----------------------------------- | -------------- |
| Pandas                              | 0.5798         |
| cuDF                                | 0.0049         |
| Pandas with `%load_ext cudf.pandas` | 0.0030         |

For this operation, cuDF is 118.4 times faster than Pandas, and Pandas with `%load_ext cudf.pandas` is 193.3 times faster than Pandas

### Writing data

We simply export the first 100.000 rows of the dataset

| Library                             | Time (seconds) |
| ----------------------------------- | -------------- |
| Pandas                              | 3.1182         |
| cuDF                                | 0.1337         |
| Pandas with `%load_ext cudf.pandas` | 3.0181         |
