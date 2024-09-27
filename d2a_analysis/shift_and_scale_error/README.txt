Here I saved the results to quantify the impact in reconstruction caused by scale and shift in the data
The scale and shift are applied as n times the std of the training set.
The std of the training set is computed in the following way:
- Compute the std of each channel of each trail in the training set
- Compute the average of the std obtained with the previous step

The scale and shift are applied in the following way:
x = (x * n_1 * std_train) + n_2 * std_train
where
	x = Original data
	std_train = standard deviation of the trainin set
	n_1 = integer
	n_2 = integer


The results are saved in the following form: 
- SN/name_dataset/full_matrix_epoch_E_rep_R_std_X_Y_Z
- SN/name_dataset/avg_matrix_epoch_E_rep_R_std_X_Y_Z
where
N = Number of the subject (1 to 9)
name_dataset = train or test
E = Epoch of training of the network used to reconstruct the signal
R = Training repetition (n.b. for each subject I have several repetition of the training)
X = Min times the std was used
Y = Max times the std was used
Z = Step in the std

full indicate the 4D matrix where for each combination of scale and shift an entire matrix of "N. trials x N.channels" is saved (i.e. for each combination of scale and shift I saved the reconstruction error of each channel and each trial)
In the avg matrix I only saved the average for each combination of scale/shift (i.e. avg_matrix[i, j] = mean(full_matrix[i, j, :, :])  

E.g. S3/train/full_matrix_epoch_80_rep_5_std_0_11_1
	This matrix will contains the results obtained with the training set of S3 and a network trained for 80 epochs.
	The standard deviation list goes from 0 to 10, with step 1 (i.e [0, 1, 2 ... , 10]). So all the combination will be (x = (x * n_1 * std_train) + n_2 * std_train):
	x = (x * 1 * std_train) + 0 * std_train
	x = (x * 1 * std_train) + 0 * std_train
	x = (x * 2 * std_train) + 0 * std_train
	x = (x * 3 * std_train) + 0 * std_train
	....
	x = (x * 10 * std_train) + 0 * std_train
	x = (x * 1 * std_train) + 1 * std_train
	x = (x * 1 * std_train) + 1 * std_train
	x = (x * 2 * std_train) + 1 * std_train
	...
	x = (x * 8 * std_train) + 10 * std_train
	x = (x * 9 * std_train) + 10 * std_train
	x = (x * 10 * std_train) + 10 * std_train
	
	P.s. note that when in the list there is a 0 and is used as n_1 in the formula above I will change it to 1 (otherwise all data will become 0)