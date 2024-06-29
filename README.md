#LA Crime Rate Investigation 


### Steps followed 

- Importing libraries 'pandas' & 'matplotlib'  
- Read dataset using pandas libraries storing variable 'crime'
    
      pd.read_csv() 
- Check for count of records

      crime.shape 
- Check for all columns with their datatypes 
    
      crime.dtypes

- Check for Null values and dropping of columns    from the dataframe.
     
      crime.isna().sum()
      df=crime.drop(labels=['modus_operandi','weapon_code','weapon_description','crime_code_2','crime_code_3','crime_code_4','cross_street'],axis=1)

- Changing Datatypes of date columns 
    
     
        df['date_reported'] = pd.to_datetime(df['date_reported'])
        df['date_occurred'] = pd.to_datetime(df['date_occurred'])
- Statistical analysis 

      df.describe(include = ['float64','int64']) 

- Filter out negative ages from the dataset and store in a new variable 

      df_age = df[df['victim_age']>0] 


Adding new month and year column in the dataset 

        df['Year'] = df['date_occurred'].dt.year
        df['Month'] = df['date_occurred'].dt.month


# Feature Understanding 
- Line plot "Crime Rate over time"
- 2nd Plot (Histogram) - "Distribution of Victims Age" 

  Crimes faced by the Different Age group of peoples
- 3rd Plot
   
   Top 20 crimes against average daily counts.
 
        plt.figure(figsize=(12,8))

        #plot 1- Line Chart 
        plt.subplot(2,2,1)
        crime_plot = df.groupby(['Year','Month']).size().plot(color='green',linewidth=2,marker='*')
        crime_plot.set_xlabel('Year - Month', fontsize=10)
        crime_plot.set_title('Crime Over Time')

        #Plot 2 - Histogram 
        plt.subplot(2,2,2)
        plt.hist(df_age['victim_age'],edgecolor='black',bins=15)
        plt.tight_layout()
        plt.xlabel('Age', fontsize=10)
        plt.title('Distribution of Victims Age')

        #Plot 3 - Horizontal Bar Chart 
        plt.figure()
        plt.barh(top_20_crimes_sorted['crime_description'],top_20_crimes_sorted['average_daily_count'],color = 'orange')
        plt.title('Top 20 Crimes')
        plt.show()

        
Charts

![Charts](https://github.com/prakashkathait/LA-Crime-Rate-Pandas/assets/166843819/997a90ce-5918-4148-a08a-5e7444624b35)

### Questions raised from the analysis 

-Q1. Show difference in reported and occured crime time - How long it takes to report?
    
    df.groupby(['crime_description'])['Time to report'].mean().sort_values().head(10)

![O1](https://github.com/prakashkathait/LA-Crime-Rate-Pandas/assets/166843819/3b320328-21e0-455d-bee8-03f26c4240bd)

Q2. Identify the top3 crimes with the highest average victim age
     
      df_age.groupby(['crime_description'])['victim_age'].mean().sort_values(ascending = False).reset_index().head(3)
![O2](https://github.com/prakashkathait/LA-Crime-Rate-Pandas/assets/166843819/4a81a992-ceb7-4876-9505-1b57ea19a3f0)

Q3. Find the area with the highest number of crimes occuring during night time.
   
    nighttime_crimes = df[(df['date_occurred'].dt.hour >=20) | (df['date_occurred'].dt.hour <=3)]

![O3](https://github.com/prakashkathait/LA-Crime-Rate-Pandas/assets/166843819/eddfd4a4-d155-4a9c-ba6e-b0a672fab6d3)

