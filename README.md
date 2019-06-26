sudo apt-get install git build-essential
sudo apt-get install opencl-headers
sudo apt-get install clinfo
sudo apt install ocl-icd-opencl-dev


After building binary file,
place files Quicksort.h  QuicksortKernels.cl to working directory

To execute Bitonic sort, file BitonicSort.cl should be placed to working dir

External merge sort requires preparation of data on file and
using it as input afterwards.
Sequence is:
# Generating input data

```
./input_generator 5000000000 input.data
```

```
Saving 5000000000 random floats (19073 MB) to file input.data
Done in 48.74 seconds (391.331 MB/s)
```

# Sorting

```
./external_merge_sort input.data output.data 1
```


