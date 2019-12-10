# Exploratory-Data-Analysis-
Exploratory Data Analysis  of Hollywood movies data set.
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib
%matplotlib inline 
import warnings
import seaborn as sns
color = sns.color_palette
import plotly.graph_objs as go
import plotly.offline as py


## neccessary importation..
import sys
import os
import inspect
print ("python"+ sys.version)
print (sys.argv[0])

## konwing python version
print(os.getcwd())
spartan= pd.read_csv("/home/prasad/Downloads/superhero-set/heroes_information.csv")
spartan.head(10)
spartan.info()
spartan.columns
list(spartan.Gender.unique())
(list(spartan.Publisher.unique()))
len(list(spartan.Publisher.unique()))
len(list(spartan.Gender.unique()))
spartan.loc[spartan['Weight'].isna()]
(spartan.loc[spartan['Publisher'].isna()])
spartan.loc[spartan['Skin color']== '-'].head()
print("missing value count in Publisher: ",spartan['Publisher'].isnull().sum())
print("missing value count in Weight: ", spartan['Weight'].isnull().sum())
print("missing value count in Skin color: ", len(spartan.loc[spartan['Skin color']=='-']))
print("missing value count in Hair color: ", len(spartan.loc[spartan['Hair color']=='-']))
print("missing value count in Eye color: ", len(spartan.loc[spartan['Eye color']=='-']))
print("missing value count in Gender: ", len(spartan.loc[spartan['Gender']=='-']))
## for printing missings
spartan.iloc[1:10,[1,2,3,6,10,9]]def missings(df):

    for column in df.columns:
        if df[column].isna().any():
            print(column," presents",df[column].isna().sum(),"number of null values.")
        if '-'in list(df[column].unique()):
            print(column,"consist",len(df.loc[df[column]=='-']),"values represented as -")

missings(spartan)

## we have created missigns fumctions and caaled at the end, it reflects all missing and values
##represented by '-'in the columns of data spartan.
spartan.columns
len(spartan.Gender)
len(spartan.Weight)
print(spartan.describe())
spartan.head()
ht_wt = spartan[['Height', 'Weight']]
ht_wt.head(5)
## replacing negative values by NaN values ,, NAN means value doesnt exist , axis =1 for columns and axis = 0 for rows 
spartan.replace(-99.0, np.nan, inplace=True)
ht_wt = spartan[['Height', 'Weight']]
ht_wt.head(5)
from sklearn.impute import SimpleImputer
imputer = SimpleImputer(strategy="median")
X = imputer.fit_transform(ht_wt)
mag= pd.DataFrame(imp, columns=ht_wt.columns)
print(mag)
spartanwithot_HW=spartan.drop(['Height','Weight'],axis=1)
spartan=pd.concat([spartanwithot_HW,mag],axis=1)
spartan.head(10)
publisher_series = spartan.Publisher.value_counts()
spartan.Race.value_counts()
publisher_series = spartan.Publisher.value_counts()
publisher_series
publisher_series.sum()
publishers = list(publisher_series.index)
print(publishers)
publications = list((publisher_series/publisher_series.sum())*100)
publications
Prasad = go.Pie(labels=publishers, values=publications)
layout=go.Layout(title='distribution of publications by publishers', height=750, width=750)
data=[Prasad]
fig=go.Figure(data=data,layout=layout)
py.iplot(fig,filename ='comic-wise-distribution-by-publishers')
spartan.Alignment.value_counts()
spartan.loc[spartan['Alignment']=='-']
spartan.Alignment.replace('-','misterias',inplace=True)
print((spartan.Alignment=='misterias').value_counts())
MIS=list(spartan.Alignment=='misterias')
tot_pub = (spartan.Publisher.value_counts().index)
tot_pub
col_names = ['Publisher', 'total_heroes', 'total_villians', 'total_neutral', 'total_unknown']
df = pd.DataFrame(columns=col_names)
df
for publisher in tot_pub:
    data = []
    data.append(publisher)
  
    data.append(len(list(spartan.loc[(spartan['Alignment']=='good') & (spartan['Publisher']==publisher),'name'])))
    data.append(len(list(spartan.loc[(spartan['Alignment']=='bad') & (spartan['Publisher']==publisher),'name'])))
    data.append(len(list(spartan.loc[(spartan['Alignment']=='neutral') & (spartan['Publisher']==publisher),'name'])))
    data.append(len(list(spartan.loc[(spartan['Alignment']=='mistarias') & (spartan['Publisher']==publisher),'name'])))
    
    df.loc[len(df)] = data
    
    df.head(10)
    t1=go.Bar(
    x=list(df.Publisher),
    y=list(df.total_heroes),
    name='total_heroes')

t2 = go.Bar(
    x=list(df.Publisher),
    y=list(df.total_villians),
    name='total_villians')

t3 = go.Bar(
    x=list(df.Publisher),
    y=list(df.total_neutral),
    name='total_neutral')

t4 = go.Bar(
    x=list(df.Publisher),
    y=list(df.total_unknown),
    name='total_unknown')

data = [t1, t2, t3, t4]
layout = go.Layout(title='Publisher-wise no. of heroes vs no. of villians', barmode='group')

fig = go.Figure(data=data, layout=layout)
py.iplot(fig, filename='heroes-vs-villians by Publishers')
spartan.replace('-', 'unknown', inplace=True)
gender_series = spartan['Gender'].value_counts()
print(gender_series)
genders = list(gender_series.index)
genders
percentage= list((gender_series/gender_series.sum())*100)
percentage
trace =go.Pie(labels= genders,values=pistriercentage)
layout = go.Layout(title='overall gender distribution',height=500,width=500)
fig=go.Figure(data=trace,layout=layout)
py.iplot(fig,filename='gender distribution')
# alignment of characters by alignment
male_df = spartan.loc[spartan['Gender']=='Male']
female_df = spartan.loc[spartan['Gender']=='Female']
trace_m = go.Bar(
    x = male_df['Alignment'].value_counts().index,
    y = male_df['Alignment'].value_counts().values,
    name="male"
)

trace_f = go.Bar(
    x = female_df['Alignment'].value_counts().index,
    y = female_df['Alignment'].value_counts().values,
    name="female"
)

data = [trace_m, trace_f]
layout = go.Layout(
    title="Distribution of Alignment by Gender",
    barmode="group"
)

fig = go.Figure(data=data, layout=layout)
py.iplot(fig, filename="alignment by gender")


***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
