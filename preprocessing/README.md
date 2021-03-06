## Preprocessing
The following steps are performed to extract raw data from acclerometer-specific formats and to save the data to a set of numpy arrays stored in HDF5 format. This step also applies auto-calibration to the raw data and aligns nonwear and sleep stage label information (when available) to the timestamps when the raw data was captured.
### a. Get raw data
Due to the scarcity of Python functions which directly load .bin/.cwa files, we use the GGIR R package to load these files to extract the raw data. Use get_raw_data_psgnewcastle.R or get_raw_data_UPenn.R to extract raw data (X,Y,Z,battery/button,light,temperature and timestamps) from .bin and .cwa files respectively. The input path is the directory containing .bin/.cwa files. The extracted data is stored in HDF5 format.

### b. Get calibration parameters and nonwear information using GGIR
Use get_calib_nonwear.R to generate calibration parameters and nonwear details from .bin/.cwa files using GGIR g.part1 function and store the intermediate results. Input includes path to directory with .bin/.cwa files, file type (.bin/.cwa) and the output path to store intermediate files.

### c. Get standard orientation for all axes
X and Y axes are dependent on the orientation of the hand while Z axis is orthogonal to it. In our 'standard' orientation, X axis values are mostly negative. If the median X axis value is positive, it implies the hand is rotated and we swap the X and Y axes by multiplying them by -1.

### d. Extract calibration parameters and nonwear information from intermediate files
Use extract_calib_nonwear.R to extract calibration parameters and nonwear details stored in intermediate files generated by Step 1b and store them in CSV files for easy access from Python.

### e. Create preprocessed dataset by applying autocalibration and aligning nonwear and label information
Use preproc_psgnewcastle.py and preproc_UPenn.py to preprocess raw data extracted in Step 1a using calibration parameters and nonwear details saved in Step 1c. Input parameters include directory path to extracted raw data from Step 1a, path to calibration parameters and path to nonwear information from Step 1c, path to label data and output path. This step applies calibration parameters to the extracted raw data and aligns nonwear and label information to the extracted data based on overlapping timestamps. The preprocessed data with calibrated X,Y & Z, battery/button,light,temperature, timestamps, nonwear and sleep stage information is stored in HDF5 format.

After performing these steps, we have the high-resolution (same as sampling frequency of raw data) cleaned data stored in HDF5 format.
