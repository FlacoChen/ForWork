#!/usr/bin/env python
# coding: utf-8

# In[1]:


import urllib.request
import urllib.error
from bs4 import BeautifulSoup
import os
import time
import random
import json
import pandas as pd
import numpy as np
def download(url,user_agent='wswp', num_retries=2):
    print('downloading: %', url)
    headers = {'User-agent': user_agent}
    request = urllib.request.Request(url, headers=headers)
    try:
        html = urllib.request.urlopen(request,timeout = 10).read()
    except:
        time.sleep(2)
        try:
            html = urllib.request.urlopen(request,timeout = 10).read()
        except:
            time.sleep(2)
            try:
                html = urllib.request.urlopen(request,timeout = 10).read()
            except:
                html = None
    return html


# In[6]:


strurl = 'http://baike.51job.com/zhiwei/all/'
html_result = download(strurl)
if html_result is None:
    print('Error')
soup = BeautifulSoup(html_result)
full = soup.find_all(attrs = 'f_list')[1]
classonesfull = full.find_all(attrs = 'lts')
count = 0
#print(classonesfull[count].find_all('a'))
classonesraw = full.find_all(attrs = 's_jname')
classones = [classone.text for classone in classonesraw]
path = r'E:\大学\实习\超级简历\笔试\\'
#print(os.listdir(path))
for classone in classones:
    classonenew = classone.replace("/", "、")
    path_new = path + str(count + 1) + "_" + classonenew
    #print(path_new)
    if classonenew not in os.listdir(path):
        os.mkdir(path_new)
    classtwosraw = classonesfull[count].find_all('a')
    classtwos = [classtwo.text for classtwo in classtwosraw]
    count_two = 0
    for classtwo in classtwos:
        link = 'http:' + classtwosraw[count_two]['href']
        #print(link)
        html_result2 = download(link)
        soup = BeautifulSoup(html_result2)
        #岗位名称：title
        title = classtwo
            
        #岗位介绍：exp
        exp = soup.find(attrs = 'j_exp').text
            
        #求职指导——岗位要求
        position_catraw = soup.find_all(attrs = 'mod')[0]
        position_cat = position_catraw.find(attrs = 'tabbar tb1 clearbox')
        position_req = position_catraw.find(attrs = 'smodel sm1')
        #print([position_req.find('li')])
        position_typesraw = position_cat.find_all('span')
        #print(position_catraw)
        position_types = [position_type.text for position_type in position_typesraw]
        count_positiontype = 0
        educations = []
        majors = []
        majorskills = []
        otherskills = []
        for position_type in position_types:
            position_list = position_req.find_all('li')[count_positiontype]
            ps = position_list.find_all('p')
            education_num = 0
            major_num = 0
            #majorskill_num = 0
            otherskill_num = 0
            for p in ps:
                if p.find('span').text == '学历要求:':
                    educations.append(p.text.replace(" ", "").replace("\r", "").replace("\n", ""))
                    education_num = 1
                elif p.find('span').text == '适合专业:':
                    majors.append(p.text.replace(" ", "").replace("\r", "").replace("\n", ""))
                    major_num = 1
                elif p.find('span').text == '其他技能要求:':
                    otherskills.append(p.text.replace(" ", "").replace("\r", "").replace("\n", ""))
                    otherskill_num = 1
            if education_num == 0:
                educations.append('学历要求:无')
            if major_num == 0:
                majors.append('适合专业:无')
            if otherskill_num == 0:
                otherskills.append('其他技能要求:无')
            majorskillstyperaw = position_list.find_all(attrs = 'm_name')
            if len(majorskillstyperaw) == 0:
                majorskills.append('专业技能要求:无')
            else:
                majorskillstype = [majorskilltype.text for majorskilltype in majorskillstyperaw]
                slevels = position_list.find_all(attrs = 's_level')
                majorskillslevel = []
                for slevel in slevels:
                    level = ''
                    #print(slevel['class'][1][2])
                    level_num = slevel['class'][1][2]
                    if level_num == '1':
                        level = '一般'
                    elif level_num == '2':
                        level = '良好'
                    elif level_num == '3':
                        level = '熟练'
                    elif level_num == '4':
                        level = '精通'
                    #print(level)
                    majorskillslevel.append(level)
                majorskill = '专业技能要求:'
                for i in range(len(majorskillstype)):
                    majorskill += majorskillstype[i] + majorskillslevel[i] + "; "
                majorskill = majorskill.rstrip()
                majorskills.append(majorskill)
            count_positiontype += 1
        #print(position_types)
        #print(educations)
        #print(majors)
        #print(majorskills)
        #print(otherskills)
        position_requirements = []
        for i in range(len(position_types)):
            position_requirement = 'a.'
            position_requirement += educations[i] + " b."
            position_requirement += majors[i] + " c."
            position_requirement += majorskills[i] + " d."
            position_requirement += otherskills[i]
            position_requirements.append(position_requirement)
        #print(position_requirements)
        #求职指导——职位工作内容
        time_cat = soup.find_all(attrs = 'mod')[1]
        time_typesraw = time_cat.find_all('span')
        time_types = [time_type.text for time_type in time_typesraw]
        requirementsraw = soup.find_all(attrs = 'job_ms')
        requirements = [requirement.text for requirement in requirementsraw]
        #print(requirements)
        #不同时间职位描述列表：time_requirements
        time_requirements = []
        count_timetype = 0
        #print(time_types)
        for time_type in time_types: 
            requirement = requirements[2 * count_timetype + 1]
            #print(requirement)
            requirement = requirement.replace(" ", "")
            requirement = requirement.replace("\r", "")
            requirement = requirement.replace("\n", "")
            #print(requirement)
            time_requirements.append(requirement)
            count_timetype += 1
        #print(time_requirements)
        #df = pd.DataFrame()
        sample = [title, exp]
        for i in range(len(position_requirements)):
            sample.append(position_requirements[i])
        for i in range(len(time_requirements)):
            sample.append(time_requirements[i])
        columns1 = ['1.岗位名称', '2.岗位介绍']
        for i in range(len(position_types)):
            columns1.append('3.岗位要求')
        for i in range(len(time_types)):
            columns1.append('4.职位工作内容')
        columns2 = ['岗位名称', '岗位介绍']
        for i in range(len(position_types)):
            columns2.append(position_types[i])
        for i in range(len(time_types)):
            columns2.append(time_types[i])
        df = pd.DataFrame([sample],                           columns = [columns1, columns2])
        path_here = path_new + "\\" + str(count_two + 1) + '_' + classtwo + '.csv'
        #print(df)
        df.T.to_csv(path_here, sep = ',', encoding = 'utf-8-sig', index = True)
        with open(path_here, 'r', encoding = 'utf-8-sig') as fin:
            data = fin.read().splitlines(True)
        with open(path_here, 'w', encoding = 'utf-8-sig') as fout:
            fout.writelines(data[1:])
        count_two += 1
    count += 1

