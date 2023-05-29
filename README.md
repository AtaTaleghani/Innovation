import pandas as pd
import numpy as np
import math
data = pd.read_excel (r'innovation.xlsx')
data = data.iloc[:, 0:6]

Before_Fouding_Year=np.zeros(data.shape[0])
for index, row in data.iterrows():
    counter = 0
    for ind, r in data.iterrows():
        if r is not row:
            if int(row['Founding Year']) > int(r['Founding Year']):
                counter += 1
    Before_Fouding_Year[index]= counter

data.insert(data.shape[1], '# Before Fouding Year', Before_Fouding_Year)
data.head()

same1=np.zeros(data.shape[0])
same2=np.zeros(data.shape[0])
same3=np.zeros(data.shape[0])

it1 = iter(range(data.shape[0]))
it2 = range(1,5)

for k in it1:
    if data.iloc[k,6] == 0:
        k = next(it1)
    for inx in range(int(data.iloc[k,6])):
        list1=[]
        list2=[]
        list3=[]
        for i in it2:
            list1.append(data.iloc[inx,i])
            list2.append(data.iloc[k,i])
        list1 = [x for x in list1 if pd.notnull(x)]
        list2 = [x for x in list2 if pd.notnull(x)]
        list3 = set(list1)&set(list2)
        list4 = sorted(list3, key = lambda h : list1.index(h))
        if len(list4) == 1:
            same1[k]=same1[k]+1
        if len(list4) == 2:
            same2[k]=same2[k]+1
        if len(list4) == 3:
            same3[k]=same3[k]+1


data.insert(data.shape[1], '# Before with 0 same keywords', data.iloc[:,6])
data.insert(data.shape[1], '# Before with 1 same keywords', same1)
data.insert(data.shape[1], '# Before with 2 same keywords', same2)
data.insert(data.shape[1], '# Before with 3 same keywords', same3)
data.head()

data.to_excel("Complete_Data.xlsx")
