# Shortcuts & other tricks to be used in the future 

### Generate a sequence of file names that follow a pattern
A great numbers of files with the same name are going to be open 

```python
# udf to create strings with file names
# fields definition: 
# pr: prefix - IE: './files/' - string
# sb: subfix: IE: '_2013p_EUSILC.csv' - string
# cr: country IE: ['LT'...] - list
def filelist(st1,st2,cr): 
    filelist = [] # strings are going to be stored into a list
    for s in cr:
        filename = st1 + s + st2
        filelist.append(filename)
    return filelist
```

### Concatenating files 
Concatenating files once opened

```python
# udf to concatenate dfs
# field definition: 
# df_list: list of dataframes storing the names of the df used
def concatenated_df(df_list):
    df_temp = pd.DataFrame()
    for file in df_list:
        df = pd.read_csv(file)
        if df_temp.shape == (0,0):
            df_temp = df
        else: 
            df_temp = pd.concat([df_temp,df])
    return df_temp
```

### Checking the number of variables for each dataframe

```python
counter = 1 
for df in df_list:
    print('dft'+ str(counter) + ': ' +str(len(df.columns)))
    counter += 1
```

### Checking the number of variables for each dataframe & df to store the results
First we store the name of the columns we want to check

```python
counter = 1
empty_list = []
df_q = pd.DataFrame(columns = ('q_index','Q1','Q2','Q3','Q4','Q5'))
for dataf in df_list:
    cols = dataf.columns
    for col in cols:
        empty_list.append(col)
df_q['q_index'] = empty_list
df_q = df_q.drop_duplicates() \
    .reset_index() \
    .drop(columns = ['index'])
```

Second we are going to store the values in the dataframe

```python
counter = 1
for dataf in df_list:
    empty_list = [] # list to store if the column is present on each dataframe (1 means stored and 0 means not stored)
    for col in dataf.columns: 
        if col in list(df_q['q_index']): 
            empty_list.append(1)
        else: 
            empty_list.append(0)
    while len(empty_list) < len(list(df_q['q_index'])): # if n columns of the independent dataframes are smaller 0 is added
        empty_list.append(0)
    df_q[df_q.columns[counter]] = empty_list
    counter += 1
