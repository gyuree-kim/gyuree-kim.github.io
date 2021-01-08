---
title: "Simple scheduler"
date: 2021-01-09
categories: data-mining
---

## Tech Tools
- Google Colab
- Python
- Google spreadsheet and docs


---
## Code Description 
### Import libraries

```python
import pandas as pd
import re
from pathlib import Path
from openpyxl import Workbook
from google.colab import files
```

### Connect with google drive
```python
from google.colab import drive
drive.mount('/content/drive')
path = '/content/drive/MyDrive/schedule'
```

### Calculate row index of input time
```python
def calculate_row(hr, min):
  if min == 0:
    row = (hr-8)*2 - 1
  elif min == 30:
    row = (hr-8)*2
  
  return row
```

### Append value to the non empty cell
```python
def append_to_cell(row, col, text, sheet):
  value = sheet.cell(row,col).value
  if value is not None:
    sheet.cell(row,col).value = value+text
  else: sheet.cell(row,col).value = value

  return
```

### Create result sheet
```python
wb = Workbook()
ws = wb.create_sheet(title='result')
```

### Initialize the result sheet
```python
for i in range(1,30):
  for j in range(1,10):
    ws.cell(i,j).value = ''
```

### Result sheet format
```python
hr = 9
num = 22-9+1

for i in range(num):
  row = 2*i+1
  ws.cell(row+1,1).value = str(hr)+':00'
  hr=hr+1

hr=9
for i in range(num):
  row = 2*i+2
  ws.cell(row+1,1).value = str(hr)+':30'
  hr=hr+1

daylist=['월','화','수','목','금','토','일']
for i in range(7):
  ws.cell(1,i+2).value = str(daylist[i])
```

### Read data
```python
from google.colab import auth
auth.authenticate_user()

import gspread
from oauth2client.client import GoogleCredentials

gc = gspread.authorize(GoogleCredentials.get_application_default())

workbook = gc.open_by_url('https://docs.google.com/spreadsheets/d/1neW4MJbPjOmngKjFf5jRx0CLra5CuaggpkKAZYWXhjg/edit#gid=0')

names = ['haneul', 'jungeun', 'judy', 'hyein', 'yewon','dayeon']
kor_names = ['하늘', '정은', '주희', '혜인', '예원', '다연']
name = 'judy'
len(names)

```

### blabla
```python
import pandas as pd

for idx in range(len(names)):
  sheet = workbook.worksheet(names[idx])

  # get_all_values gives a list of rows.
  rows = sheet.get_all_values()
  print(rows)

  # Convert to a DataFrame and render.
  pd.DataFrame.from_records(rows)
  df = pd.DataFrame(rows)
  df.columns = df.iloc[0] #same as rows[0] 

  for i in range(1,4): #available time list
    for j in range(7): #Monday to Sunday
      time = df.iloc[i][j]
      if time != 'x':
        #calculate start row index and end row index
        print(time, int(time[0]+time[1]), int(time[3]+time[4]), int(time[6]+time[7]), int(time[9]+time[10]) )      
        start_hour = int(time[0]+time[1])
        start_min = int(time[3]+time[4])
        end_hour = int(time[6]+time[7])
        end_min = int(time[9]+time[10])
        
        start_row = calculate_row(start_hour, start_min)
        end_row = calculate_row(end_hour, end_min)

        #write names in result sheet
        for k in range(start_row,end_row):
          append_to_cell(k+1,j+2,str(' '+ kor_names[idx]),ws)
```

### save file
```python
wb.save(path+'/this-week.xls')
```

---