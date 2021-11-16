# dataframe
## 基本
### 匯入與輸出資料
```python
import pandas as pd
df = pd.read_csv("name") #read_excel #轉檔 : data_xls.to_csv('1.csv', encoding='utf-8')
#usecols=[1,3],sep='\t',header=0,skiprows=3,encoding='gbk'
df = df[['Name','ID']] #讀取特定行
df.to_csv("NAME",index=False)
```
### column改名(rename)
```python
df.rename(columns={'ID':'Name'},inplace=True)
```
### 增加新columns (空)
```python
df = pd.DataFrame(df,columns=['Name','ID','New _one'])

df["Empty_1"] = ""
df["Empty_2"] = np.nan
df['Empty_3'] = pd.Series() 
```
![](https://i.imgur.com/Eo5bQEY.png)

### 去除指定行 (del)(drop)
```python
# 有時不行
crm.drop(columns=[5,6,7],inplace=True)
#inplace = True：不創建新的對象，直接對原始對象進行修改；
#inplace = False：對數據進行修改，創建並返回新的對象承載其修改結果。

#一次刪一行
del crm[5]
del crm[6]
del crm[7]
```
### 重新編號(index)
```python
df.index = range(len(df))
df.reset_index(inplace=True)
```
### 連接兩資料(concat)
```python
df = pd.concat([df1,df2])
```
### 讓數字column變str
```python
crm.columns = crm.columns.astype(str)
```
### 提取資料
```python=
app.loc[0:9,:]  #提取0~9前十筆資料的全部欄位
app.loc[:,['track_name','price']]  #提取track_name,price欄位的所有資料
app.loc[0,:]  #提取第一筆資料的所有欄位
app.loc[:,'track_name']  #提取track_name欄位的所有資料
app.loc[0,'track_name']  #提取第一筆資料的track_name欄位
```


## 資料處理

### NAN (notna)
* 去除NAN
```python
df = df[df['Name'].notna()]
```
* 取代NAN
```python
density['total counts'] = density['total counts'].fillna(0)
df.fillna(method="ffill") #propagate non-null values forward or backward
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
#比較自己資料
ans = df[df['Name'].isin(set(df['ID']))]
ans = df[df['ID'] != df['Name']] #篩選掉自己NAME=ID者
#與他人比較
ans = df[df['Name'].isin(set(df1['ID']))]
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
### 按照某些條件整理數據 (groupby)
* 基礎groupby
```python
df = df.astype(str).groupby(['Name'],as_index=False).agg(list) #按照某行整理數據
```
```python
group = tf.groupby('chromosome') #選擇某column
group.size()  #輸出每項數量
group.get_group('chr2L') #取得各項成果
```
![](https://i.imgur.com/9CM4QsD.png)
* 計算各項次數
```python=
freq =mapping.groupby(['input_id']).size()  # freq = mapping['input_id'].value_counts() # freq =mapping.groupby(['input_id']).count() 
print(freq) # freq.index.name = 'id'      freq.index[3]      freq[3]
dict_freq = freq.to_dict() #df變字典
dict_freq['G1']
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
### 取代 (replace)
```python
df.replace(to_replace, value)
```
### 取出特定資料 (regex)
```python
crm[9] = crm[8].str.extract(r'target=FB:(\w*):', expand=False).str.strip()
```
### 比對資料並取代 (update) (map)
```python=
df_is_name_1to1['Systematic Name'] = df_is_name_1to1['Systematic Name'].map(name_map_1to1.set_index('Name/Alias')['Systematic Name']).fillna(df['Systematic Name'])
df['c3'].update(df['c3'].map(df.set_index('c1')['c2'])) #用c1來比對，c2來更換
df['c3'] = df['c3'].map(df.set_index('c1')['c2']) #.fillna(df['c3'])
```
https://stackoverflow.com/questions/52986145/comparing-and-replacing-column-items-pandas-dataframe

### 資料篩選 (==,!=,<,>)
* 顯示True or False
```python=
df["Gender"] == "Male"
```
* 顯示出符合數據
```python=
fliter = (df["Gender"] == "Male")
df[fliter] #等於df[df["Gender"] == "Male"]
```
* 多個條件
```python=
mask1 = df["Team"] != "Marketing"
mask2 = df["Start Date"] < "1980-01-01"
df[(mask1 & mask2)]  #and
df[(mask1 ｜ mask2)]  #or
```
* 比較大小後，取代數值
```python=
df.loc[df.pos_mean < df.CDS_start,'count5'] = df['evenly_rc'] #.loc[條件,欲更換值] = 更換值
```

### **apply**
* 每欄增加（可用其他df)
```python=
freq =mapping.groupby(['input_id']).size()  # freq = mapping['input_id'].value_counts() # freq =mapping.groupby(['input_id']).count() 
print(freq) # freq.index.name = 'id'      freq.index[3]      freq[3]
dict_freq = freq.to_dict()
dict_freq['G1']

mapping['freq'] = mapping['input_id'].apply(lambda x:dict_freq[x])
density['total counts'] = density['Gene name'].apply(lambda x:dict_counts.get(x)) #字典中不包含所有Gene name
```
### map
```python=
df1["result"] = df1['name'].map(dict(zip(df['name'],df['result']))).fillna('') #dict(zip(df['name'],df['result'])) 變為dict
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
