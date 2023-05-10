<h2 align="center">  МИНИСТЕРСТВО НАУКИ И ВЫСШЕГО ОБРАЗОВАНИЯ РОССИЙСКОЙ ФЕДЕРАЦИИ ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ БЮДЖЕТНОЕ ОБРАЗОВАТЕЛЬНОЕ УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ «САХАЛИНСКИЙ ГОСУДАРСТВЕННЫЙ УНИВЕРСИТЕТ» </h2>
<div align="center">
<h3>Институт естественных наук и техносферной безопасности
<br>
Кафедра информатики
<br>
Половников Владислав Олегович</h3>

<br>
<br>
“Программирование на языке Python”
<br>
01.03.02 Прикладная математика и информатика</h3>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

<h3 align="center">Южно-Сахалинск
<br>
2023г.
</h3>
<hr>
</div>

<h3 align="center">Задание</h3>
<h4>Найти любой источник данных в интернете, загрузить с помощью pandas, посчитать основные статистики, построить произвольные графики в matplotlib</h4>

<h2 align="center">Решение</h2>

![img1](/images/1.png)

![img2](/images/2.png)

![img3](/images/3.png)

![img4](/images/4.png)

```python
import pandas as pd
import tkinter as tk
from tkinter import ttk
from collections import Counter
import matplotlib.pyplot as plt
import numpy as np

salaries = pd.read_csv('ds_salaries.csv')

# Создание окна для статистики
root = tk.Tk()
root.title("Статистика")
root.geometry("1500x800")
root.minsize(1000, 500)

# Создание таблицы данных
table = ttk.Treeview(root, columns=("work_year", "experience_level","employment_type","job_title","salary","salary_currency","salary_in_usd","employee_residence","remote_ratio","company_location","company_size"), show="headings")
table.configure(height=25)

table.heading("work_year", text="Год")
table.heading("experience_level", text="Опыт работы")
table.heading("employment_type", text="Тип занятости")
table.heading("job_title", text="Должность")
table.heading("salary", text="Зарплата")
table.heading("salary_currency", text="Валюта")
table.heading("salary_in_usd", text="В долларах")
table.heading("employee_residence", text="Гражданство")
table.heading("remote_ratio", text="Объем удаленной работы")
table.heading("company_location", text="Местонахождение компании")
table.heading("company_size", text="Размер компании")

table.column("work_year", width=100,stretch=True)
table.column("experience_level", width=100,stretch=True)
table.column("employment_type", width=100,stretch=True)
table.column("job_title", width=300,stretch=True)
table.column("salary", width=100,stretch=True)
table.column("salary_currency", width=100,stretch=True)
table.column("salary_in_usd", width=100,stretch=True)
table.column("employee_residence", width=100,stretch=True)
table.column("remote_ratio", width=100,stretch=True)
table.column("company_location", width=100,stretch=True)
table.column("company_size", width=100,stretch=True)



def insert_data_in_table():
    for i in range(0,len(salaries)):
        work_year = salaries.loc[i, 'work_year']
        experience_level = salaries.loc[i, 'experience_level']
        employment_type = salaries.loc[i, 'employment_type']
        job_title = salaries.loc[i, 'job_title']
        salary = salaries.loc[i, 'salary']
        salary_currency = salaries.loc[i, 'salary_currency']
        salary_in_usd = salaries.loc[i, 'salary_in_usd']
        employee_residence = salaries.loc[i, 'employee_residence']
        remote_ratio = salaries.loc[i, 'remote_ratio']
        company_location = salaries.loc[i, 'company_location']
        company_size = salaries.loc[i, 'company_size']
        table.insert(parent='', index='end', values=(work_year,experience_level,employment_type,job_title,salary,salary_currency,salary_in_usd,employee_residence,remote_ratio,company_location,company_size))


def statistic():
#------------------Опыт работы-----------------------------------#
    result_text = "По опыту работы: "
    length = len(salaries['experience_level'].tolist())
    exp_level = Counter(salaries['experience_level'].tolist())
    for item, value in exp_level.items():
        result = round(value/length * 100)
        result_text += "\n" + str(item) + ": " + str(result) + "%"
    label = tk.Label(root, text=result_text, font=12);
    label.pack(side=tk.LEFT)

#------------------Тип занятости---------------------------------#
    result_text = "По типу занятости: "
    length = len(salaries['employment_type'].tolist())
    employment_type = Counter(salaries['employment_type'].tolist())
    for item, value in employment_type.items():
        result = round(value/length * 100)
        result_text += "\n" + str(item) + ": " + str(result) + "%"
    label = tk.Label(root, text=result_text, font=12);
    label.pack(side=tk.LEFT)

#---------------Максимальная зарплата------------------------------#
    result_text = "Максимальная зарплата:  "
    max_salary = max(salaries['salary_in_usd'].tolist())
    result_text += '\n' + str(max_salary)
    label = tk.Label(root, text=result_text, font=12);
    label.pack(side=tk.LEFT)

#--------------Максимальная зарплата по гражданству----------------#
    label = tk.Label(root, text="Максимальная зарплата по гражданствам: ", font=12);
    label.pack(side=tk.LEFT)
    max_salaries = salaries.groupby('employee_residence')['salary_in_usd'].max()
    table1 = ttk.Treeview(root, columns=("salary", "residence"), show="headings")
    table1.heading("residence", text="Гражданство")
    table1.heading("salary", text="Зарплата")
    table1.column("residence", width=100,stretch=True)
    table1.column("salary", width=100,stretch=True)
    for item, value in max_salaries.items():
        table1.insert('','end',values=(item,value))
    table1.configure(height=8)
    table1.pack(side=tk.LEFT)



def Plot():
#------------------Окно с годами работы---------------------------#
    fig, ax = plt.subplots()
    years = Counter(salaries['work_year'].tolist())
    x = years.keys()
    y = years.values()
    ax.bar(x, y, width=1, edgecolor="white", linewidth=0.7)
    ax.set_xticks(range(int(min(x)), int(max(x)) + 1))
    plt.title('Год работы')
    plt.xlabel('Года')
    plt.ylabel('Количество')

#------------------Окно с размерами компаний----------------------#
    fig, ax = plt.subplots()
    explode = (0,0,0.2)
    exp = Counter(salaries['company_size'].tolist())
    sizes = exp.values()
    labels = exp.keys()
    ax.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
    plt.title('Размеры компаний')

#----Окно с максимальными зарплатами и опытом работы по годам-----#
    max_salaries = salaries.groupby(['experience_level', 'work_year'])['salary_in_usd'].max()
    species = salaries['work_year'].unique()
    exp_levels = salaries['experience_level'].unique()
    exp_salaries = {}
    for experience_level in exp_levels:
        exp_salaries[experience_level] = [max_salaries[experience_level, work_year] for work_year in species]

    x = np.arange(len(species))
    width = 0.15
    multiplier = 0

    fig, ax = plt.subplots(layout='constrained')
    for attribute, measurement in exp_salaries.items():
        offset = width * multiplier
        rects = ax.bar(x + offset, measurement, width, label=attribute)
        ax.bar_label(rects, padding=3)
        multiplier += 1

    ax.set_ylabel('Зарплата')
    ax.set_title('Максимальная зарплата по опыту работы')
    ax.set_xticks(x + width * 1.5, species)
    ax.legend(loc='upper left', ncol=5)
    ymax = max(max(v) for v in exp_salaries.values()) *1.1
    ymin = min(min(v) for v in exp_salaries.values()) *0.9
    ax.set_ylim(ymin,ymax)

    plt.show()

def create_UI():
    insert_data_in_table()
    table.pack()
    statistic()
    Plot()
    root.mainloop()



if __name__ == '__main__':
    create_UI()


```
