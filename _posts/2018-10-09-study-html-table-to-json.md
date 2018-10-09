---
layout: post
title:  "html table 크롤링 json 파일로 저장"
subtitle:   "json 파일형식"
categories: study
tags: crawling
comments: true
---

- 이렇게 하는게 맞는지는 모르겠으나...
- 일단 데이터가 임시로 필요해서 SK 엔카 홈페이지를 크롤링했다. 

- table 크롤링 하기 
- json 파일 형태로 전환해서 저장하기



```python
from selenium import webdriver
from bs4 import BeautifulSoup
from urllib.request import urlopen

dr =  webdriver.Chrome('C:/Users/YOONHOI/Desktop/chromedriver')
dr.implicitly_wait(3)
dr.get('http://www.encar.com/dc/dc_carsearchlist.do?carType=kor&searchType=model&TG.R=A#!')

html = dr.page_source
soup = BeautifulSoup(html,'html.parser')


# 사실 이 밑은 홈페이지 소스코드 보고 반 노가다로 긁었당... 
table_div = soup.find(id = "rySch_result")
tables = table_div.find_all("table")
basic_table=tables[1]# 일반매물
trs = basic_table.find_all('tr') # tr = 자동차 1대
del(trs[0])

idx=-1
li=[]
for row in trs:
    
    idx=idx+1
    a=dict()
    a = {'idx' : idx} # trs 1번째  =  인덱스 0 
    
    tds = row.find_all('td') # 첫번째 정보 나누기
    
    img_link=tds[0].find('img').attrs['src'] #이미지 링크를 얻었다
    a['img']=img_link # 딕셔너리에 저장
    
    spans = tds[1].find_all('span')
    for temp in spans : 
        a[temp.attrs['class'][0]]=temp.text
            
    for i in range(2,6):
        a[tds[i].attrs['class'][0]]=tds[i].text
        
    li.append(a)
```



```python
# 필요없는 데이터는 날려주기

new_li = []
for x in li:
    try:
        if 'hotMark' in x : del x['hotMark']
        if 'hMark' in x : del x['hMark']
        if 'ins' in x : del x['ins']
        if 'ass' in x : del x['ass']
        if 'detail' in x : del x['detail']
        new_li.append(x)
    except :
        pass
```

```python
import json
from collections import OrderedDict

with open('used_car.json','w',encoding = "utf-8") as make_file : 
    json.dump(new_li,make_file,ensure_ascii=False,indent = "\t")
```

