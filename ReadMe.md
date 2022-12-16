## Weather and Collision Data Set

In this project I have a dataset named Motor Vehicle Collisions- Crashes available on NYC Open data webiste. This dataset covers data from all over New York State and this code here gives some sample data to show a preliminary view of the dataset.
Then further I have merged this dataset with the weather station neareest to the location of the crash site.

```python
import gc
import csv

fn = 'Motor_Vehicle_Collisions_-_Crashes.csv'
f = open(fn, 'r')
reader = csv.reader(f)
n =0
for row in reader:
    print(row)
    if n==2:
        break
    n+=1
print(n)
gc.collect()
```

    ['CRASH DATE', 'CRASH TIME', 'BOROUGH', 'ZIP CODE', 'LATITUDE', 'LONGITUDE', 'LOCATION', 'ON STREET NAME', 'CROSS STREET NAME', 'OFF STREET NAME', 'NUMBER OF PERSONS INJURED', 'NUMBER OF PERSONS KILLED', 'NUMBER OF PEDESTRIANS INJURED', 'NUMBER OF PEDESTRIANS KILLED', 'NUMBER OF CYCLIST INJURED', 'NUMBER OF CYCLIST KILLED', 'NUMBER OF MOTORIST INJURED', 'NUMBER OF MOTORIST KILLED', 'CONTRIBUTING FACTOR VEHICLE 1', 'CONTRIBUTING FACTOR VEHICLE 2', 'CONTRIBUTING FACTOR VEHICLE 3', 'CONTRIBUTING FACTOR VEHICLE 4', 'CONTRIBUTING FACTOR VEHICLE 5', 'COLLISION_ID', 'VEHICLE TYPE CODE 1', 'VEHICLE TYPE CODE 2', 'VEHICLE TYPE CODE 3', 'VEHICLE TYPE CODE 4', 'VEHICLE TYPE CODE 5']
    ['09/11/2021', '2:39', '', '', '', '', '', 'WHITESTONE EXPRESSWAY', '20 AVENUE', '', '2', '0', '0', '0', '0', '0', '2', '0', 'Aggressive Driving/Road Rage', 'Unspecified', '', '', '', '4455765', 'Sedan', 'Sedan', '', '', '']
    ['03/26/2022', '11:45', '', '', '', '', '', 'QUEENSBORO BRIDGE UPPER', '', '', '1', '0', '0', '0', '0', '0', '1', '0', 'Pavement Slippery', '', '', '', '', '4513547', 'Sedan', '', '', '', '']
    2
    


# Number of rows with null data in either of the prominent rows
In this dataset there a ot of empyty spaces and requires a lot a lot of data preprocessing, so if we try to remove the rows with NULL values we are left with no data. To overcome this problem I have tried to find the most prominent fields in this dataset so that we can move on further. Crash Date, Crash Time and zipcode are identified as most import fields together so as to give us the basic information of the collision.
Here in the cell below I have tried counting how many rows have atleast one of the prominent fields of my dataset as NULL also the index of these rows. Then these values are written to a new file named Acollisions.csv.


```python
import csv

fn = 'Motor_Vehicle_Collisions_-_Crashes.csv'
f = open(fn, 'r')
reader = csv.reader(f)
n = 0
nc = 0
ncl = list()

for row in reader:
    if row[0]==''or row[1]=='' or row[3]=='':

        #if row[0][6:10] != '2021' and row[0][6:10] != '2022' and row[0][6:10] != '2020':
        nc+=1
        ncl.append(n)
        #print(n,nc,ncl)
    #print(n)
    n+=1

ncl = set(ncl)

```


```python
import csv
fn = 'Motor_Vehicle_Collisions_-_Crashes.csv'
f = open(fn, 'r')
reader = csv.reader(f)

f2 = open('Acollision.csv','w')
f2.write('')
f2.close()

f2 = open('Acollision.csv','a')
writer = csv.writer(f2,delimiter=',',lineterminator='\n')
ncl = set(ncl)


n=0
for row in reader:
    if n not in ncl:
        if n == 0:
            l = []
            for i in range(len(row)):
                if i == 0:
                    continue
                elif i == 1:
                    l.append('CRASH DATETIME')
                else:
                    l.append(row[i])
            writer.writerow(l)
        else:
            l = []
            for i in range(len(row)):
                if i == 0:
                    s = row[i]
                elif i == 1:
                    s = s+' '+row[i]
                    l.append(s)
                else:
                    l.append(row[i])
            writer.writerow(l)
            
    n+=1
print(n)

```

    1952268
    


```python
import pandas as pd
df = pd.read_csv('Acollision.csv')
df.isnull().sum()
```

    C:\Users\admin\AppData\Local\Temp\ipykernel_3552\1250550710.py:2: DtypeWarning: Columns (2) have mixed types. Specify dtype option on import or set low_memory=False.
      df = pd.read_csv('Acollision.csv')
    




    CRASH DATETIME                         0
    BOROUGH                                0
    ZIP CODE                               0
    LATITUDE                           35419
    LONGITUDE                          35419
    LOCATION                           35419
    ON STREET NAME                    280765
    CROSS STREET NAME                 281319
    OFF STREET NAME                  1065403
    NUMBER OF PERSONS INJURED             11
    NUMBER OF PERSONS KILLED              23
    NUMBER OF PEDESTRIANS INJURED          0
    NUMBER OF PEDESTRIANS KILLED           0
    NUMBER OF CYCLIST INJURED              0
    NUMBER OF CYCLIST KILLED               0
    NUMBER OF MOTORIST INJURED             0
    NUMBER OF MOTORIST KILLED              0
    CONTRIBUTING FACTOR VEHICLE 1       4551
    CONTRIBUTING FACTOR VEHICLE 2     213120
    CONTRIBUTING FACTOR VEHICLE 3    1263880
    CONTRIBUTING FACTOR VEHICLE 4    1326650
    CONTRIBUTING FACTOR VEHICLE 5    1340236
    COLLISION_ID                           0
    VEHICLE TYPE CODE 1                 8720
    VEHICLE TYPE CODE 2               255791
    VEHICLE TYPE CODE 3              1266416
    VEHICLE TYPE CODE 4              1327161
    VEHICLE TYPE CODE 5              1340367
    dtype: int64




```python
f2 = open('Acollision.csv','r')
reader= csv.reader(f2)
n=0
for row in reader:
    print(row)
    if n ==2:
        break
    n+=1
n=0
for row in reader:
    n+=1
print(n)    
```

    ['CRASH DATETIME', 'BOROUGH', 'ZIP CODE', 'LATITUDE', 'LONGITUDE', 'LOCATION', 'ON STREET NAME', 'CROSS STREET NAME', 'OFF STREET NAME', 'NUMBER OF PERSONS INJURED', 'NUMBER OF PERSONS KILLED', 'NUMBER OF PEDESTRIANS INJURED', 'NUMBER OF PEDESTRIANS KILLED', 'NUMBER OF CYCLIST INJURED', 'NUMBER OF CYCLIST KILLED', 'NUMBER OF MOTORIST INJURED', 'NUMBER OF MOTORIST KILLED', 'CONTRIBUTING FACTOR VEHICLE 1', 'CONTRIBUTING FACTOR VEHICLE 2', 'CONTRIBUTING FACTOR VEHICLE 3', 'CONTRIBUTING FACTOR VEHICLE 4', 'CONTRIBUTING FACTOR VEHICLE 5', 'COLLISION_ID', 'VEHICLE TYPE CODE 1', 'VEHICLE TYPE CODE 2', 'VEHICLE TYPE CODE 3', 'VEHICLE TYPE CODE 4', 'VEHICLE TYPE CODE 5']
    ['09/11/2021 9:35', 'BROOKLYN', '11208', '40.667202', '-73.8665', '(40.667202, -73.8665)', '', '', '1211      LORING AVENUE', '0', '0', '0', '0', '0', '0', '0', '0', 'Unspecified', '', '', '', '', '4456314', 'Sedan', '', '', '', '']
    ['12/14/2021 8:13', 'BROOKLYN', '11233', '40.683304', '-73.917274', '(40.683304, -73.917274)', 'SARATOGA AVENUE', 'DECATUR STREET', '', '0', '0', '0', '0', '0', '0', '0', '0', '', '', '', '', '', '4486609', '', '', '', '', '']
    1345606
    


```python
gc.collect()
```


For simplicity, I only wanted to use data ranging from year 2020-2022. The part of code below takes only data from the range 2020-2022 and writes them to a new file.


```python
f = open('Acollision.csv','r')
reader= csv.reader(f)

f2 = open('collision_2020_2022.csv','w')
f2.write('')
f2.close()

f2 = open('collision_2020_2022.csv','a')
writer = csv.writer(f2,delimiter=',',lineterminator='\n')
n=0
for row in reader:
    if n==0:
        writer.writerow(row)
    elif row[0][6:10]=="2020" or row[0][6:10]=="2021" or row[0][6:10]=="2022":
        writer.writerow(row)
    n+=1
print(n)
f2.close()
```

    1345672
    


```python
f2 = open('collision_2020_2022.csv','r')
reader= csv.reader(f2)
n=0
for row in reader:
    print(row)
    if n ==2:
        break
    n+=1
n=0
for row in reader:
    n+=1
print(n)    
```

    ['CRASH DATETIME', 'BOROUGH', 'ZIP CODE', 'LATITUDE', 'LONGITUDE', 'LOCATION', 'ON STREET NAME', 'CROSS STREET NAME', 'OFF STREET NAME', 'NUMBER OF PERSONS INJURED', 'NUMBER OF PERSONS KILLED', 'NUMBER OF PEDESTRIANS INJURED', 'NUMBER OF PEDESTRIANS KILLED', 'NUMBER OF CYCLIST INJURED', 'NUMBER OF CYCLIST KILLED', 'NUMBER OF MOTORIST INJURED', 'NUMBER OF MOTORIST KILLED', 'CONTRIBUTING FACTOR VEHICLE 1', 'CONTRIBUTING FACTOR VEHICLE 2', 'CONTRIBUTING FACTOR VEHICLE 3', 'CONTRIBUTING FACTOR VEHICLE 4', 'CONTRIBUTING FACTOR VEHICLE 5', 'COLLISION_ID', 'VEHICLE TYPE CODE 1', 'VEHICLE TYPE CODE 2', 'VEHICLE TYPE CODE 3', 'VEHICLE TYPE CODE 4', 'VEHICLE TYPE CODE 5']
    ['09/11/2021 9:35', 'BROOKLYN', '11208', '40.667202', '-73.8665', '(40.667202, -73.8665)', '', '', '1211      LORING AVENUE', '0', '0', '0', '0', '0', '0', '0', '0', 'Unspecified', '', '', '', '', '4456314', 'Sedan', '', '', '', '']
    ['12/14/2021 8:13', 'BROOKLYN', '11233', '40.683304', '-73.917274', '(40.683304, -73.917274)', 'SARATOGA AVENUE', 'DECATUR STREET', '', '0', '0', '0', '0', '0', '0', '0', '0', '', '', '', '', '', '4486609', '', '', '', '', '']
    209995
    
This part confirms if the data belongs to the required range. 
## Date-Time Range


```python
import datetime,csv
fn = 'collision_2020_2022.csv'

f = open(fn, 'r')
reader = csv.reader(f)
minval = None
maxval = None
n = 0

for row in reader:
    if n > 0:
        dto = None
        dts = row[0]
        try:
            dto = datetime.datetime.strptime(dts,"%m/%d/%Y %H:%M" )
        except Exception as e:
            print(e)
        if dto is not None:
            if maxval is None or dto > maxval:
                maxval = dto
            elif minval is None or dto < minval:
                minval = dto
    n+=1
print(minval, maxval)
          
```

    2020-01-01 00:00:00 2022-12-09 23:50:00
    


```python
import pandas as pd
df = pd.read_csv('collision_2020_2022.csv')
df.isnull().sum()
```

Here we can see the LONGITUDE and LATITUDE fields have null values


    CRASH DATETIME                        0
    BOROUGH                               0
    ZIP CODE                              0
    LATITUDE                           6227
    LONGITUDE                          6227
    LOCATION                           6227
    ON STREET NAME                    82584
    CROSS STREET NAME                 82675
    OFF STREET NAME                  127413
    NUMBER OF PERSONS INJURED             0
    NUMBER OF PERSONS KILLED              0
    NUMBER OF PEDESTRIANS INJURED         0
    NUMBER OF PEDESTRIANS KILLED          0
    NUMBER OF CYCLIST INJURED             0
    NUMBER OF CYCLIST KILLED              0
    NUMBER OF MOTORIST INJURED            0
    NUMBER OF MOTORIST KILLED             0
    CONTRIBUTING FACTOR VEHICLE 1      1226
    CONTRIBUTING FACTOR VEHICLE 2     50523
    CONTRIBUTING FACTOR VEHICLE 3    191484
    CONTRIBUTING FACTOR VEHICLE 4    204750
    CONTRIBUTING FACTOR VEHICLE 5    208327
    COLLISION_ID                          0
    VEHICLE TYPE CODE 1                2808
    VEHICLE TYPE CODE 2               73140
    VEHICLE TYPE CODE 3              192760
    VEHICLE TYPE CODE 4              204988
    VEHICLE TYPE CODE 5              208382
    dtype: int64


This part of code helps us to find distinct zipcodes and stores them in a dictionary.

```python
import csv

fn = 'c_collision_2020_2022.csv'

f = open(fn, 'r')
reader = csv.reader(f)
n = 0
zip_dic = dict()
for row in reader:
    if n>0:
        if row[2] in zip_dic:
            zip_dic[row[2]]+=1
        else:
            zip_dic[row[2]]=1
    n+=1
    

```


```python
print(len(zip_dic),n)
```

    220 209935
    


```python
len(l)
```

Since we have zipcodes, we can fill in the missing values of longitudes and latitudes using these. File 'Zip.csv' contains zipcodes and their central longitudes and latitutes. We use this data and merge them with our collision data to fill the empty values in Longitude and Latitiude columns.

```python
import csv

fn = 'Zip.csv'
zip_code=dict()
f = open(fn, 'r')
reader = csv.reader(f)
n = 0
for row in reader:
    if str(row[2]) in zip_code:
        continue
    else:
        zip_code[str(row[2])]= [row[0],row[1]]
    
    n+=1
print(n,len(zip_code))
```

    33145 33145
    


```python

```


```python
n=0


for i in zip_code:
    print(i,zip_code[i])
    if n==3:
        break
    n+=1
print(n)

if '11249' in zip_code:
    print('hai')


```

    ZIP ['LAT', 'LNG']
    00601 ['18.180555', '-66.749961']
    00602 ['18.361945', '-67.175597']
    00603 ['18.455183', '-67.119887']
    3
    


```python
f = open('c_collision_2020_2022.csv','r')
reader= csv.reader(f)

f2 = open('final_collision_2020_2022.csv','w')
f2.write('')
f2.close()

f2 = open('final_collision_2020_2022.csv','a')
writer = csv.writer(f2,delimiter=',',lineterminator='\n')
n=0
for row in reader:
    if row[3]=="" or row[4]=="":
        if row[2] in zip_code:
            l = []
            for i in range(len(row)):
                if i == 3:
                    s = zip_code[row[2]]
                    l.append(s[0])
                elif i ==4:
                    s = zip_code[row[2]]
                    l.append(s[1])
                else:
                    l.append(row[i])
            writer.writerow(l)
        else:
            continue
    else:
        writer.writerow(row)
    n+=1
print(n)
```

    209961
    


```python
import pandas as pd
df1 = pd.read_csv('final_collision_2020_2022.csv')
df1.isna().sum()
```




    CRASH DATETIME                        0
    BOROUGH                               0
    ZIP CODE                              0
    LATITUDE                              0
    LONGITUDE                             0
    LOCATION                           6188
    ON STREET NAME                    82547
    CROSS STREET NAME                 82638
    OFF STREET NAME                  127354
    NUMBER OF PERSONS INJURED             0
    NUMBER OF PERSONS KILLED              0
    NUMBER OF PEDESTRIANS INJURED         0
    NUMBER OF PEDESTRIANS KILLED          0
    NUMBER OF CYCLIST INJURED             0
    NUMBER OF CYCLIST KILLED              0
    NUMBER OF MOTORIST INJURED            0
    NUMBER OF MOTORIST KILLED             0
    CONTRIBUTING FACTOR VEHICLE 1      1224
    CONTRIBUTING FACTOR VEHICLE 2     50505
    CONTRIBUTING FACTOR VEHICLE 3    191394
    CONTRIBUTING FACTOR VEHICLE 4    204656
    CONTRIBUTING FACTOR VEHICLE 5    208231
    COLLISION_ID                          0
    VEHICLE TYPE CODE 1                2807
    VEHICLE TYPE CODE 2               73113
    VEHICLE TYPE CODE 3              192669
    VEHICLE TYPE CODE 4              204894
    VEHICLE TYPE CODE 5              208286
    dtype: int64




```python
import pandas as pd
df1 = pd.read_csv('f1.csv')

```


```python
df1.nunique()
```




    STATION                         1
    DATE                        13215
    BackupLatitude                  1
    BackupLongitude                 1
    BackupName                      1
    HourlyAltimeterSetting        167
    HourlyPresentWeatherType      101
    HourlySkyConditions          5033
    HourlyVisibility               47
    HourlyWindDirection            38
    HourlyWindSpeed                31
    dtype: int64




```python
df5 = pd.read_csv('f5.csv')
df5.nunique()
```




    STATION                         1
    DATE                        12517
    BackupLatitude                  1
    BackupLongitude                 1
    HourlyAltimeterSetting        165
    HourlyPresentWeatherType       84
    HourlySkyConditions          3962
    HourlyVisibility               45
    HourlyWindDirection            38
    HourlyWindSpeed                33
    dtype: int64




```python
f= open('f1.csv','r')
reader = csv.reader(f)
for row in reader:
    weather_head=row
    break
print(len(weather_head))
```

    10
    
This function finds the distance between two cordinates.

```python
from math import radians, cos, sin, asin, sqrt
def haversine(lon1, lat1, lon2, lat2):
    """
    Calculate the great circle distance in kilometers between two points 
    on the earth (specified in decimal degrees)
    """
    # convert decimal degrees to radians 
    lon1, lat1, lon2, lat2 = map(radians, [lon1, lat1, lon2, lat2])

    # haversine formula 
    dlon = lon2 - lon1 
    dlat = lat2 - lat1 
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * asin(sqrt(a)) 
    r = 6371 # Radius of earth in kilometers. Use 3956 for miles. Determines return value units.
    return c * r
```

This function returns data from the data of nearest weather station with same date and hour of both merged with data of the collision dataset.
```python
def nearest_ws(rlat,rlong,l_fn,data):
    nwsd = dict()
   
    for i in l_fn:
        mn=999999999
        f = open(i,'r')
        reader = csv.reader(f)
        j=0
        for row in reader:
            if j>0:
                #print(row[3])
                d = haversine(float(row[3]),float(row[2]),rlong,rlat)
                if d<=mn:
                    mn=d
                    wf=i
                break
            j+=1
        f.close()
    f = open(wf,'r')
    j=0
    reader = csv.reader(f)
    for row in reader:
        if j>0:
            s1 = row[1]
            d1 = s1.split()
            mdy1 = d1[0].split('/')
            h1 = int(d1[1].split(':')[0]) 
            s2 = data[0]
            d2 = s2.split()
            mdy2 = d2[0].split('/')
            h2 = int(d2[1].split(':')[0])
            if int(mdy1[0]) == int(mdy2[0]) and int(mdy1[1]) == int(mdy2[1]) and int(mdy1[2]) == int(mdy2[2][2:]) and h1 == h2:
                y = data+row
                return y
            else:
                return data+['','','','','','','','','','',]
        j+=1
```

Here in this code we call the above function and get the merged data and writes it to a new file.
 
```python
ff = open('final_collision_2020_2022.csv','r')

f2 = open('weather_collision_2020_2022.csv','w')
f2.write('')
f2.close()

f2 = open('weather_collision_2020_2022.csv','a')
writer = csv.writer(f2,delimiter=',',lineterminator='\n')

reader = csv.reader(ff)
l = ['f4.csv','f5.csv']
n=0
for row in reader:
    if n==0:
        writer.writerow(weather_head)
    else:
        #print(row[4],row[3])
        weather_station = nearest_ws(float(row[4]),float(row[3]),l,row)
        writer.writerow(weather_station)
    n+=1
        
```


```python

```


```python

```


```python

```
