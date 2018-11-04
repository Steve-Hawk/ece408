# 3D Convolution

## Objective

The purpose of this lab is to implement a 3D convolution using constant memory for the kernel and 3D shared memory tiling.

## Prerequisite Before starting this lab, make sure that:

- You have completed the "Tiled Matrix Multiplication" MP
- You have completed all week 4 videos

## Instructions

Edit the code to implement a 3D convolution with a `3x3x3` kernel in constant memory and a 3D shared-memory tiling.

Edit the code to launch the kernel you implemented. The function should launch 3D CUDA grid and blocks, where each thread is responsible for computing a single element of the output.

Answer the questions found in the questions tab.

### Algorithm Specification

You will be implementing the following 3D convolution.

```c
for z_out = 0 to z_size - 1:
  for y_out = 0 to y_size - 1:
    for x_out = 0 to x_size - 1: {
      let res = 0;
      for z_mask = - MASK_RADIUS to MASK_RADIUS:
        for y_mask = - MASK_RADIUS to MASK_RADIUS:
          for x_mask = - MASK_RADIUS to MASK_RADIUS:
            let z_in = z_out +  z_mask;
            let y_in = y_out + y_mask;
            let x_in = x_out + x_mask;
            // Pad boundary with 0
            if (z_in >= 0 && z_in < z_size &&
                y_in >= 0 && y_in < y_size &&
                x_in >= 0 && x_in < x_size) then
               res += mask[z_mask + MASK_RADIUS][y_mask + MASK_RADIUS][x_mask + MASK_RADIUS] * in[z_in][y_in][x_in]
           }
      out[z_out][y_out][x_out] = res;
    }
```

- The kernel size is fixed to `3x3x3`, given `MASK_WIDTH = 3` and `MASK_RADIUS = 1`.
- Halo elements should be read as `0`.
- You should support input data of any size.

Note that the input and output size is the same.

### Other Notes

The raw format of the input data is a flattened array, where the first three elements are the `z_size`, `y_size`, and `x_size` respectively. For example, a `5x4x3` input array will look like

```
float inputData[] = { 5.0, 4.0, 3.0, ... < 60 floats > }
```

A point `(z,y,x)` may be accessed at `z * (y_size * x_size) + y * (x_size) + x`.

The template code reads the first three elements into `z_size`, `y_size`, and `x_size`. You will need to copy the rest of the data to the device.

Likewise, the result needs to have the sizes prepended for WebGPU to check your result correctly. The template code does that as well, but you must copy the data into outputData from the fourth element on.

Remember that you can get a pointer to the fourth element of the array with `&arry[3]`.

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
