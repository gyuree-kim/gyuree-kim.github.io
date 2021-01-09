---
title: "Simple scheduler"
date: 2021-01-09
categories: data-mining
---

여러 명의 팀원이 있을 때, **각자 본인이 가능한 시간을 입력하면 서로 가능한 시간대를 엑셀파일로 한 눈에 확인할 수 있는** 프로그램을 만들었습니다. 이전에 근무했던 스타트업에서 영업시간 내에 30분 단위로 담당자가 1명씩 배정되어야해서 만들었던 프로그램입니다. 이 외 활용도는 다음과 같이 나열할 수 있고, 본인 취향대로 코드를 조금씩 수정하면 활용도가 더 높아질 수 있겠죠!

- 모임 시간 정할 때 모든 사람이 가능한 시간 찾기
- 여러 사람이 겹치지 않게 시간 배분하기
- (활용) 이름을 카운트하는 함수 추가 --> 각 사람이 얼마큼 기여했는지 확인하기


## Tech Tools
- Google Colab
- Python
- Google spreadsheet and docs

## Input Sheet
img..

## Output Shhet
img..

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
필요한 라이브러리를 import 합니다. 처음에 `xlrd`, `xlwt`도 사용해봤는데 `xlwt`에는 각 셀의 value을 get 할 수 있는 `cell(row, col).value` 옵션이 없어서 `openpyxl`을 사용했습니다. 혹시 방법을 못 찾은 것일수도 있으니 한 번 시도해보세요. 


### Connect with google drive
```python
from google.colab import drive
drive.mount('/content/drive')
path = '/content/drive/MyDrive/schedule'
```
`drive.mount`를 사용하여 구글 드라이브와 연결합니다. 실행을 시키면 구글 로그인으로 연결되는데, 로그인 이후 나오는 인증코드를 입력해주면 됩니다.
인증이 끝나면 구글 드라이브 내의 모든 파일에 쉽게 접근이 가능해집니다.(유레카!)


### Calculate row index of input time
```python
def calculate_row(hr, min):
  if min == 0:
    row = (hr-8)*2 - 1
  elif min == 30:
    row = (hr-8)*2
  
  return row
```
영업시간이 9시 00분부터 22시 30분이고 30분 단위로 행 인덱스가 증가합니다.
시간을 입력받으면 몇 번째 row인지 계산해주는 함수입니다.

### Append value to the non empty cell
```python
def append_to_cell(row, col, text, sheet):
  value = sheet.cell(row,col).value
  if value is not None: #important!!
    sheet.cell(row,col).value = value+text
  else: sheet.cell(row,col).value = value

  return
```
입력 파일에서 각 시트를 차례대로 돌면서 각 사람이 가능한 시간대를 탐색한 후, 결과 파일에 업데이트합니다. 이 때, 중복되는 시간이 있을 경우 해당 셀이 단순히 `override`되지 않도록 append 함수를 작성했습니다. 중요한 점은 업데이트할 때 셀이 비어있는 케이스를 처리해줘야하는 것입니다. 처리해주지 않으면 에러가 발생합니다. 에러 메세지가 훌륭하니 확인하고 고쳐봐도 좋습니다.

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
결과 시트 파일에 1열에는 9:00부터 22:30까지 30분 단위로 입력하고, 1행에는 요일을 입력해줍니다.

### Read data
```python
from google.colab import auth
auth.authenticate_user()

import gspread
from oauth2client.client import GoogleCredentials

gc = gspread.authorize(GoogleCredentials.get_application_default())

workbook = gc.open_by_url('https://docs.google.com/spreadsheets/d/1neW4MJbPjOmngKjFf5jRx0CLra5CuaggpkKAZYWXhjg/edit#gid=0')

names = name['name1', 'name2', 'name3', 'name4', 'name5','name6']
kor_names = ['이름1', '이름2', '이름3', '이름4', '이름5', '이름6']
```
위의 input sheet 이미지처럼 읽어야 할 파일의 각 시트는 영어 이름으로 작성되어 있습니다. 한국어로 작성했을 때 시트 이름에서 읽기는 되는데 해당 이름으로 작성은 실패해서, 영어 이름으로 읽어온 뒤 한국어 이름으로 작성하도록 변경했습니다. 입력 파일은 openpyxl을 사용했던 결과 파일과 다른 방식인 `gspread`를 이용했습니다. 

### Write result
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
wb.save(path+'/this-week')
```
결과 파일을 `this-week`라는 이름으로 저장해줍니다. 파일 확장자도 함께 입력해도 됩니다. ex) `this-week.xls`

---