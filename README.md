# non-natice_contact
Non-native contact visualization
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import glob
# Define the directory containing the files
dir_path = "/work9/haont/copy-update/500/gln/trial1/contact"


# Define the pattern for the file names
file_pattern = "contactmindist_18_*.xvg"

# Get a list of all file names that match the pattern
file_names = [f for f in os.listdir(dir_path) if f.startswith('contactmindist_18_') and f.endswith('.xvg')]

# Sort the file names based on the integer value of 'abc'
file_names = sorted(file_names, key=lambda x: int(x.split("_")[2].split(".")[0]))

# Load the data from each file
data = pd.DataFrame()
for file_name in file_names:
    file_path = os.path.join(dir_path, file_name)
    file_data = pd.read_csv(file_path, delim_whitespace=True, skiprows=24, header=None, usecols=[1])
    file_data.rename(columns={1: file_name}, inplace=True)
    data = pd.concat([data, file_data], axis=1)
# Define the directory containing the files
dir_path = "/work9/haont/copy-update/500/gln/trial5/contact"


# Define the pattern for the file names
file_pattern = "contactmindist_19_*.xvg"

# Get a list of all file names that match the pattern
file_names = [f for f in os.listdir(dir_path) if f.startswith('contactmindist_19_') and f.endswith('.xvg')]

# Sort the file names based on the integer value of 'abc'
file_names = sorted(file_names, key=lambda x: int(x.split("_")[2].split(".")[0]))

# Load the data from each file
data19 = pd.DataFrame()
for file_name in file_names:
    file_path = os.path.join(dir_path, file_name)
    file_data = pd.read_csv(file_path, delim_whitespace=True, skiprows=24, header=None, usecols=[1])
    file_data.rename(columns={1: file_name}, inplace=True)
    data19 = pd.concat([data19, file_data], axis=1)
df = data.merge(data19.iloc[:, 0:], left_index=True, right_index=True)
# Define the cutoff distance
cutoff = 3.0

# Create a new matrix to store the non-native contacts
non_native = np.zeros((df.shape[0]-1, df.shape[1]))

# Loop over each row of the data (except the first row)
for i in range(1, df.shape[0]):
    for j in range( df.shape[1]):
        if df.iloc[i, j] < cutoff:
            non_native[i-1, j] = 1

  
print(non_native.shape)
labley = ['0','100','200','300','400','500']
lablex = ['Ligand A - Resid1', 'Ligand A - Resid170', 'Ligand A - Resid340', 'Ligand A - Resid510', 'Ligand A - Resid680', 'Ligand B - Resid1', 'Ligand B - Resid170', 'Ligand B - Resid340', 'Ligand B - Resid510', 'Ligand B - Resid680', 'Ligand B - Resid850']
sns.heatmap(non_native, cmap='coolwarm')
plt.ylabel('Time')
plt.title('Non-Native Contacts')

x_tick_positions = np.linspace(0, non_native.shape[1], len(lablex))
y_tick_positions = np.linspace(0, non_native.shape[0], len(labley))

plt.xticks(x_tick_positions, labels=lablex, rotation='vertical')
plt.yticks(y_tick_positions, labels=labley, rotation='horizontal')
plt.tight_layout()
plt.savefig('contact.png')
plt.clf()
