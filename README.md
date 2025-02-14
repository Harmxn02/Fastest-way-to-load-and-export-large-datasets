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

### Writing data

TO-DO
