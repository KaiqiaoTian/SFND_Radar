# 2D CFAR Implementation README

2D CFAR

    Determine the number of Training cells for each dimension. Similarly, pick the number of guard cells.
    Slide the cell under test across the complete matrix. Make sure the CUT has margin for Training and Guard cells from the edges.
    For every iteration sum the signal level within all the training cells. To sum convert the value from logarithmic to linear using db2pow function.

    Average the summed values for all of the training cells used. After averaging convert it back to logarithmic using pow2db.
    Further add the offset to it to determine the threshold.
    Next, compare the signal under CUT against this threshold.
    If the CUT level > threshold assign it a value of 1, else equate it to 0.

The process above will generate a thresholded block, which is smaller than the Range Doppler Map as the CUTs cannot be located at the edges of the matrix due to the presence of Target and Guard cells. Hence, those cells will not be thresholded.

To keep the map size same as it was before CFAR, equate all the non-thresholded cells to 0.


# FFT Operation
FFT Operation

    Implement the 1D FFT on the Mixed Signal
    Reshape the vector into Nr*Nd array.
    Run the FFT on the beat signal along the range bins dimension (Nr)
    Normalize the FFT output.
    Take the absolute value of that output.
    Keep one half of the signal
    Plot the output
    There should be a peak at the initial position of the target



## Selection of Training graud cells and offset

Due to repeated experimentation and observing the output, the following hyperparameters were chosen:


    Number of Training cells in range dimension (Tr) = 10
    Number of Training cells in doppler dimension (Td) = 8
    Number of Guard cells in range dimension (Gr) = 4
    Number of Guard cells in doppler dimension (Gd) = 4
    Offset = 1.4


These were primarily chosen because of the walkthrough video, but also because of repeated experimentation to see how the performance varies when we slowly diverge away from the initial hyperparameters chosen.  The initial hyperparameters chosen were already good enough, so they remain in the final version of this project.

## Dealing with the edge cases

Finally, in a vectorised manner we simply use indexing to suppress all of the edges of the output by simply examining how much the halfway point of the rows and columns would be in the proposed mask and setting those locations in the output mask to 0 accordingly.  By using indexing, we leave the remaining elements intact.