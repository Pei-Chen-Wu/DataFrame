# DataFrame
DataFrame Summary
## 基本
### 匯入與輸出資料
```python
import pandas as pd
df = pd.read_csv("name")
#usecols=[1,3],sep='\t',header=0,skiprows=3
df = df[['Name','ID']] #讀取特定行
df.to_csv("NAME",index=False)
```
### column改名(rename)
```python
df.rename(columns={'ID':'Name'},onplace=True)
```
### 增加新columns (空)
```python
df = pd.DataFrame(df,columns=['Name','ID','New _one'])
```
### 去除指定行 (del)(drop)
```python
# 有時不行
crm.drop(columns=[5,6,7])
#一次刪一行
del crm[5]
del crm[6]
del crm[7]
```
### 重新編號(index)
```python
df.index = range(len(df))
```
### 連接兩資料(concat)
```python
df = pd.concat([df1,df2])
```

## 資料處理

### 去除NAN (notna)
```python
df = df[df['Name'].notna()]
```
### 去除指定列(drop)
```python
a._stst_axis.values.tolist() = [1,9,15,21] #指定數據的index
df = df.drop(labels=a._stst_axis.values.tolist(),axis=0)
```
### 重複數據(duplicated)
```python
df[df.duplicated()] #顯示重複數據
df = df.drop_duplicates() #去除重複數據
```
### 尋找相同物件(isin)
```python
ans = df[df['Name'].isin(set(df['ID']))]
ans = df[df['ID'] != df['Name']] #篩選掉自己NAME=ID者
```
### 拆解資料(split)
```python
df1= df[0].str.split('|',expand=True).stack() #拆開並翻轉
df1= df.reset_index(level=1,drop=True).rename('Name') #重新編號index與rename
df = df.join(df1).drop(0,axis=1) #加入df1並刪除原column
```
### 按照某行排序(sort_values) 單格排序(sorted)
```python
df = df.sort_values(by=['Name'])
for i in range(len(df)):
    df['ID'][i] = sorted(df['ID'][i])
```
### 按照某行整理數據 (groupby)
```python
df = df.astype(str).groupby(['Name'],as_index=False).agg(list)
```
### 符合條件添加至新datafram (append)
```python
df1 = pd.DataFrame()
for i in range(len(df)):
    if len(df['Name'][i]) == 1 :
        df1 = df1.append(df[i:i+1])
```
### 更改整行文字
```python
crm[0] = 'chr' + crm[0]
```
### 取出特定資料 (regex)
```python
crm[9] = crm[8].str.extract(r'target=FB:(\w*):', expand=False).str.strip()
```

## 補充
### 去除list
```python
#函式方法
del flatten(t):
    return [item for sublist in t for item in sublist]
#暴力拆
for i in range(len(df)):
    df['ID'][i] = df['ID'][i][2:-2]
```
### 對答案 (equals)(compare)
```python
b.equals(a) #回傳True or False
b.compare(a) #列出不同數據
```
