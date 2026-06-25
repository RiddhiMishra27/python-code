# python-code
mport pandas as pd
import numpy as np
import matplotlib.pyplot as plt
df=pd.read_csv("data.csv")
print(df.head())
print(df.columns)
def period(hour):
    if 8<=hour<=10:
        return "peak"
    elif 5<=hour<=8:
        return "peak"
    else:
        return "offpeak"
df['hour']=pd.to_datetime(df['order_time']).dt.hour
df['period']=df['hour'].apply(period)
delay=df.groupby('period')['delay_mins'].sum()
print(delay)
peak=delay.get('peak',0)
offpeak=delay.get('offpeak',0)
print("diff=",peak - offpeak)
delay.plot(kind='bar')
plt.title("peak vs offpeak delay")
plt.ylabel("avg delay")
plt.show()
weather_delay=df.groupby('weather')['delay_mins'].median()
print(weather_delay)
weather_delay.plot(kind='bar')
plt.title("weather vs delay")
plt.ylabel("median delay")
plt.show()
low_exp=df[df['rider_experience']<2]['delay_mins']
high_exp=df[df['rider_experience']>4]['delay_mins']
print(low_exp,high_exp)
print("diff=",high_exp - low_exp)
plt.boxplot([low_exp,high_exp])
plt.title("rider experience")
plt.show()
city_rate=df.groupby('city')['on_time'].mean()*100
city_rate.barplot()
plt.title("city ontime rate")
plt.show()
df['month']=pd.to_datetime(df['delivery_date']).dt.month
monthly_rate=df.groupby('monthly')['delay_mins'].mean()
monthly_rate.barplot()
plt.title("monthly delay trend")
plt.show()
vehicle_rate=df.groupby('vehicle_type')['delay_mins'].mean()
vehicle_rate.barplot()
plt.title("vehicle comparison")
plt.show()
worst_city=city_rate.idxmin()
print("lowest performance city=", worst_city)
print("recommendation:increase riders and route optimization during peak hours")
