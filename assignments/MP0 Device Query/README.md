# Lab Tour with Device Query

## Objective
The purpose of this lab is to get you familiar with using the submission system for this course and the hardware used.

## Instructions

Click on the Code tab and then read the code written. Do not worry if you do not understand all the details of the code (the purpose is to get you familiar with the submission system). Once done reading, click the "Compile & Run" button.

The submission system will automatically switch to the compile-and-run results that will also be available through the **Attempts** tab. There, you will be able to see a summary of your attempt.

The `Timer Output` section has 4 columns:

- *Kind* describes the type of device whose execution time is being measured. This is specified with the first argument supplied to `wbTimer_start`. In this example, the GPU execution time is measured.
- *Location* indicates where (file::line_number) in the user code was the `wbTimer_start` called. In this example, you should be able to find the line at which `wbTimer_start` is called in the 'Program Code' section with the line_number.
- *Time* (ms) shows the amount of time it took for the device to run your assignment program, measured in milliseconds.
- *Message* gives the strong that you passed to `wbTimer_start` in the second argument. This is typically a comment about the nature of the measurement.
- Similarly, you will see the following information under the Logger Output section.

The `Logger Output` section has 3 columns:

- *Level* is the level specified when calling the `wbLog` function (indicating the severity of the event),
- *Location* describes the `function::line_number` of the wbLog call, and
- *Message* which is the message specified for the `wbLog` function

The `Timer Output` or `Logger Output` seconds are hidden, if no timing or logging statements occur in your program.

We log the hardware information used for this course --- the details which will be explained in the first few lectures.

- GPU card's name
- GPU computation capabilities
- Maximum number of block dimensions
- Maximum number of grid dimensions
- Maximum size of GPU memory
- Amount of constant and share memory
- Warp size

All results from previous attempts can be found in the Attempts tab. You can choose any of these attempts for submission for grading. Note that even though you can submit multiple times, only your last submission will be reflected in the course database.

After completing this lab, and before proceeding to the next one, you will find it helpful to watch the tutorial videos if you have not done so.

Click the 'Questions' tab and answer the questions to the best of your ability. Some of the questions will be better answered after the first few lectures. You could use Google to find most of the answers. No points are assigned to questions in this assignment.

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
