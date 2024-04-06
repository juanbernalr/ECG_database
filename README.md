License: [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/ "CC-BY-4.0")

When using this database (repository), cite the original publication:
```
@inproceedings{reference1,
 author = "Bernal-Romero, Juan Carlos and Ramírez-Cortés, Juan Manuel",
 booktitle = "", 
 title = "An open-access database of lead-I ECG signals with practical variability scenarios", 
 year = "2024",
 volume = "",
 number = "",
 pages = "",
 keywords = "ECG database; Lead-I ECG; Intra-user variability; ECG biometric system; ECG morphology variability",
 doi=""}
```

------------
# **An open-access database of lead-I ECG signals with practical variability scenarios**

This project aims to consolidate and provide a freely and openly available database of I-lead ECG signals with variability scenarios for the development, research, and evaluation of biometric and ambulatory diagnostic systems.

This database is consolidated from an own database and mostly from public databases with open-access licenses. The analysis, processing, and selection of the ECG signals were performed manually, signal by signal from each of the databases.

There are lead-I ECG recordings from **562** individuals. Everyone has **10** recordings in different variability scenarios of **10 seconds** duration each. Moreover, the variability scenarios are as follows: physical activities, mental stress (cognitive and emotional activities), change over time, the effect of commercial drugs for heart disease, and effects of heart medical antecedents. These variability scenarios are practical in the real world when developing access control systems based on ECG signals.
The **5620 raw recordings** have a sampling rate of **125 Hz** and a resolution of **10 bits** for a supply voltage of 0 to 3.3 V to the acquisition system.

------------
## Labeled information for each user

The files **Database_information.ods** and **Database_information.xlsx** have the same information. This information consists of sex, age, variability scenarios, acquisition postures, heart medical antecedents, heart rhythm, smoking habits and observations of the ECG signals for each subject identified with a number from **1** to **562**.

------------
## Read database from Python

```python
def read_DB_downloaded():
    import csv
    directory = 'file_directory_path_for_the_database\subject_'
    subjects = 562
    ECG_DB = []
    for i in range(0,subjects):
        filename =  directory +  str(i+1) + '.csv'
        file = open(filename, "r")
        data = list(csv.reader(file,delimiter=",", quoting=csv.QUOTE_NONNUMERIC))
        file.close()
        ECG_DB.append([row for row in data])
    return ECG_DB
```
```python
def read_DB_GitHub():
    import pandas as pd
    subjects = 562
    ECG_DB = []
    for i in range(0,subjects):
        filename = "https://raw.githubusercontent.com/juanbernalr/ECG_database/main/data_ECG/subject_" + str(i+1) + ".csv"
        data = pd.read_csv(filename,delimiter=",",header=None)
        data = data.values.tolist()
        ECG_DB.append([row for row in data])
    return ECG_DB
```
Functions to obtain the matrix with the lead-I ECG signals from the database:

```python
ECG_DB = read_DB_GitHub()  
ECG_DB = read_DB_downloaded()
```

------------
## Read database from Matlab

```
function [ECG_DB] = read_DB_downloaded()
    directory1 = "file_directory_path_for_the_database\subject_"
    subjects = 562;
    ECG_DB = cell(562,10);
    for i=1:subjects
        filename = strcat(directory,num2str(i),'.csv');
        data = readmatrix(filename); % Or csvread()
        for o=1:10
            ECG_DB{i,o} = data(o,:);
        end
    end
end
```

```
function [ECG_DB] = read_DB_GitHub()   
    subjects = 562;
    ECG_DB = cell(562,10);
    for i=1:subjects
        filename = strcat("https://raw.githubusercontent.com/juanbernalr/ECG_database/main/data_ECG/subject_",num2str(i),'.csv');
        data = readmatrix(filename); % Or csvread()
        for o=1:10
            ECG_DB{i,o} = data(o,:);
        end
    end
end
```

Functions to obtain the matrix with the lead-I ECG signals from the database:

```
ECG_DB = read_DB_GitHub();
ECG_DB = read_DB_downloaded();
```
The result of the reading (Python or Matlab) corresponds to a **matrix of 562 rows by 10 columns** where each cell is an observation of 10 seconds or 1250 samples for a sampling frequency of 125 Hz. Each row corresponds to each individual and each column corresponds to a variability scenario or observation.

------------

## Example for plotting ECG observations from the database
### Pyhton
```python
import matplotlib.pyplot as plt
subject = 112
observation = 8
t = [n*(1/125) for n in range(0,1250)]  
plt.plot(t,ECG_DB[subject-1][observation-1],linewidth=1.5,color='blue')
plt.xlabel('Time [s]')
plt.ylabel('Voltage [mV]')
```

![](https://raw.githubusercontent.com/juanbernalr/ECG_database/main/Images/Python.png)

### Matlab
```
subject = 112;
observation = 8;
t = 0:1/125:10-(1/125);
plot(t,ECG_DB{subject,observation},'b','LineWidth',1.5);
xlabel("Time [s]");
ylabel("Amplitude [mV]");
```
![](https://raw.githubusercontent.com/juanbernalr/ECG_database/main/Images/Matlab.png)

------------
