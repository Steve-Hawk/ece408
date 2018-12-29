# List Scan - Parallel Scan

## Objective

The purpose of this lab is to implement one or more kernels and their associated host code to perform parallel scan on a 1D list. The scan operator used will be addition. You should implement the work- efficient kernel discussed in lecture. Your kernel should be able to handle input lists of arbitrary length. However, for simplicity, you can assume that the input list will be at most 2,048 * 2,048 elements.

## Prerequisites

Before starting this lab, make sure that:

- You have completed all week 4 lecture videos
- You have completed all week 5 lecture videos
- You have completed the List Reduction Lab


## Instruction

The boundary condition can be handled by filling 'identity value (0 for sum)' into the shared memory of the last block when the length is not a multiple of the thread block size.

You will need to launch multiple kernels to complete the parallel scan as discussed in the lecture.

Edit the code in the code tab to perform the following:

- allocate device memory
- copy host memory to device
- initialize thread block and kernel grid dimensions
- invoke CUDA kernel
- copy results from device to host
- deallocate device memory
- implement the work-efficient scan kernel to generate per-block scan array and store the block sums into an auxiliary block sum array.
- use shared memory to reduce the number of global memory accesses, handle the boundary conditions when loading input list elements into the shared memory
- reuse the kernel to perform scan on the auxiliary block sum array to translate the elements into accumulative block sums. Note that this kernel will be launched with only one block.
- implement the kernel that adds the accumulative block sums to the appropriate elements of the per-block scan array to complete the scan for all the elements.

Instructions about where to place each part of the code is demarcated by the `//@@` comment lines.

## Suggestions

- The system's autosave feature is not an excuse to not backup your code and answers to your questions regularly.
- If you have not done so already, watch the tutorial videos.
- Do not modify the template code provided -- only insert code where the `//@@` demarcation is placed
- Develop your solution incrementally and test each version thoroughly before moving on to the next version
- Do not wait until the last minute to attempt the lab.
- If you get stuck with boundary conditions, grab a pen and paper. It is much easier to figure out the boundary conditions there.
- Implement the serial CPU version first, this will give you an understanding of the loops
- Get the first dataset working first. The datasets are ordered so the first one is the easiest to handle
- Make sure that your algorithm handles non-regular dimensional inputs (not square or multiples of 2). The slides may present the algorithm with nice inputs, since it minimizes the conditions. The datasets reflect different sizes of input that you are expected to handle
- Make sure that you test your program using all the datasets provided (the datasets can be selected using the dropdown next to the submission button)
- Check for errors: for example, when developing CUDA code, one can check for if the function call succeeded and print an error if not via the following macro:

    ```
    #define wbCheck(stmt) do {                                                    \
            cudaError_t err = stmt;                                               \
            if (err != cudaSuccess) {                                             \
                wbLog(ERROR, "Failed to run stmt ", #stmt);                       \
                wbLog(ERROR, "Got CUDA error ...  ", cudaGetErrorString(err));    \
                return -1;                                                        \
            }                                                                     \
        } while(0)
    ```

    An example usage is `wbCheck(cudaMalloc(...))`. A similar macro can be developed while programming OpenCL code.


## Local Development

While not required, the library used throughout the course can be downloaded from [Github](https://github.com/abduld/libwb). The library does not depend on any external library (and should be cross platform), you can use make to generate the shared object file (further instructions are found on the Github page). Linking against the library would allow you to get similar behavior to the web interface (minus the imposed limitations). Once linked against the library, you can launch your program as follows:

```
./program -e <expected_output_file> -i <input_file_1>,<input_file_2> -o <output_file> -t <type>
```

The `<expected_output_file>` and `<input_file_n>` are the input and output files provided in the dataset. The `<output_file>` is the location you'd like to place the output from your program. The `<type>` is the output file type: vector, matrix, or image. If an MP does not expect an input or output, then pass none as the parameter.

In case of issues or suggestions with the library, please report them through the [issue tracker](https://github.com/abduld/libwb/issues) on Github.
