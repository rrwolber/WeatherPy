# WeatherPy



```python
# Observations:
#     -Seasonal polarity can be observed on Lat vs Temp as the southern latitudes 
#     (currently in summer) show higher temperatures on average than northern latitudes,
#     with those points in the middle showing the highest and most uniform temperatures.

#     -Latitudes closer to the northern pole have a higher variance in temperature
    
#     -There does not appear to any obvious correlation between latitude and
#     either wind speed, cloudiness, or humidity.


import seaborn
import pandas as pd
import matplotlib.pyplot as plt 
import numpy as np
import requests as req
import json
from citipy import citipy
import random
import datetime

now = datetime.datetime.now()
```


```python
def getCities(x,y):
    randlong = round((random.randint(-180,179) + random.random()),2) 
    randlat = round((random.randint(-90,89) + random.random()), 2)
    city = citipy.nearest_city(randlat, randlong)
    if (city.city_name, city.country_code) not in x and (city.city_name, city.country_code) not in y:
        x.append((city.city_name, city.country_code))
    else:
        getCities(x,y)
    return(x)

weather_data = []
cityList = []

print('Beginning Data Retrieval')
print('-------------------------')

for i in range(10):
    url = "http://api.openweathermap.org/data/2.5/weather"
    params = {'appid': "25df918b1b747a46beb7e27fc1f85b9d",
              'q': '',
              'units': 'imperial'}    
    cityCount = 0 
    citySet = []
    counter = 0
    while cityCount < 50:
        getCities(citySet,cityList)
        cityCount = cityCount + 1
    

    for city, code in citySet:
        params["q"] = f'{city},{code}'
        linkcity = city.replace(" ", "_")
        response = req.get(url, params=params)
        response_json = response.json()
        if response.status_code == 404:
            counter = counter
            getCities(citySet, cityList)   
        else:
            for data in response_json:
                relevantInfo = {"City":response_json["name"], 
                                "Cloudiness":response_json["clouds"]["all"], 
                                "Country":response_json["sys"]["country"],
                                "Humidity":response_json["main"]["humidity"], 
                                "Lat":response_json["coord"]["lat"], 
                                "Lng":response_json["coord"]["lon"],
                                "Max Temp (F)":response_json["main"]["temp_max"], 
                                "Wind Speed":response_json["wind"]["speed"]}
            weather_data.append(relevantInfo)
            counter = counter + 1
            print(f'Processing Record {counter} of Set {i+1} | {city}') 
            print(f'http://api.openweathermap.org/data/2.5/weather?q={linkcity},{code}&units=imperial')
            
    cityList = cityList + citySet
    
    
            
city_data_df = pd.DataFrame(weather_data)
city_data_df.to_csv("City_Data.csv")


```

    Beginning Data Retrieval
    -------------------------
    Processing Record 1 of Set 1 | tashtagol
    http://api.openweathermap.org/data/2.5/weather?q=tashtagol,ru&units=imperial
    Processing Record 2 of Set 1 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?q=beringovskiy,ru&units=imperial
    Processing Record 3 of Set 1 | challans
    http://api.openweathermap.org/data/2.5/weather?q=challans,fr&units=imperial
    Processing Record 4 of Set 1 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?q=ushuaia,ar&units=imperial
    Processing Record 5 of Set 1 | sao joao do paraiso
    http://api.openweathermap.org/data/2.5/weather?q=sao_joao_do_paraiso,br&units=imperial
    Processing Record 6 of Set 1 | albany
    http://api.openweathermap.org/data/2.5/weather?q=albany,au&units=imperial
    Processing Record 7 of Set 1 | bilma
    http://api.openweathermap.org/data/2.5/weather?q=bilma,ne&units=imperial
    Processing Record 8 of Set 1 | san patricio
    http://api.openweathermap.org/data/2.5/weather?q=san_patricio,mx&units=imperial
    Processing Record 9 of Set 1 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?q=sao_joao_da_barra,br&units=imperial
    Processing Record 10 of Set 1 | santiago
    http://api.openweathermap.org/data/2.5/weather?q=santiago,ph&units=imperial
    Processing Record 11 of Set 1 | ayan
    http://api.openweathermap.org/data/2.5/weather?q=ayan,ru&units=imperial
    Processing Record 12 of Set 1 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?q=klaksvik,fo&units=imperial
    Processing Record 13 of Set 1 | east london
    http://api.openweathermap.org/data/2.5/weather?q=east_london,za&units=imperial
    Processing Record 14 of Set 1 | dasoguz
    http://api.openweathermap.org/data/2.5/weather?q=dasoguz,tm&units=imperial
    Processing Record 15 of Set 1 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?q=ostrovnoy,ru&units=imperial
    Processing Record 16 of Set 1 | atuona
    http://api.openweathermap.org/data/2.5/weather?q=atuona,pf&units=imperial
    Processing Record 17 of Set 1 | pudozh
    http://api.openweathermap.org/data/2.5/weather?q=pudozh,ru&units=imperial
    Processing Record 18 of Set 1 | manono
    http://api.openweathermap.org/data/2.5/weather?q=manono,cd&units=imperial
    Processing Record 19 of Set 1 | butaritari
    http://api.openweathermap.org/data/2.5/weather?q=butaritari,ki&units=imperial
    Processing Record 20 of Set 1 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?q=mount_gambier,au&units=imperial
    Processing Record 21 of Set 1 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?q=yellowknife,ca&units=imperial
    Processing Record 22 of Set 1 | carballo
    http://api.openweathermap.org/data/2.5/weather?q=carballo,es&units=imperial
    Processing Record 23 of Set 1 | vestmanna
    http://api.openweathermap.org/data/2.5/weather?q=vestmanna,fo&units=imperial
    Processing Record 24 of Set 1 | kodiak
    http://api.openweathermap.org/data/2.5/weather?q=kodiak,us&units=imperial
    Processing Record 25 of Set 1 | busselton
    http://api.openweathermap.org/data/2.5/weather?q=busselton,au&units=imperial
    Processing Record 26 of Set 1 | torbay
    http://api.openweathermap.org/data/2.5/weather?q=torbay,ca&units=imperial
    Processing Record 27 of Set 1 | arman
    http://api.openweathermap.org/data/2.5/weather?q=arman,ru&units=imperial
    Processing Record 28 of Set 1 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?q=tuktoyaktuk,ca&units=imperial
    Processing Record 29 of Set 1 | mabini
    http://api.openweathermap.org/data/2.5/weather?q=mabini,ph&units=imperial
    Processing Record 30 of Set 1 | dikson
    http://api.openweathermap.org/data/2.5/weather?q=dikson,ru&units=imperial
    Processing Record 31 of Set 1 | batagay
    http://api.openweathermap.org/data/2.5/weather?q=batagay,ru&units=imperial
    Processing Record 32 of Set 1 | hami
    http://api.openweathermap.org/data/2.5/weather?q=hami,cn&units=imperial
    Processing Record 33 of Set 1 | berdigestyakh
    http://api.openweathermap.org/data/2.5/weather?q=berdigestyakh,ru&units=imperial
    Processing Record 34 of Set 1 | kapaa
    http://api.openweathermap.org/data/2.5/weather?q=kapaa,us&units=imperial
    Processing Record 35 of Set 1 | prieska
    http://api.openweathermap.org/data/2.5/weather?q=prieska,za&units=imperial
    Processing Record 36 of Set 1 | provideniya
    http://api.openweathermap.org/data/2.5/weather?q=provideniya,ru&units=imperial
    Processing Record 37 of Set 1 | thompson
    http://api.openweathermap.org/data/2.5/weather?q=thompson,ca&units=imperial
    Processing Record 38 of Set 1 | tura
    http://api.openweathermap.org/data/2.5/weather?q=tura,ru&units=imperial
    Processing Record 39 of Set 1 | faanui
    http://api.openweathermap.org/data/2.5/weather?q=faanui,pf&units=imperial
    Processing Record 40 of Set 1 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?q=bambous_virieux,mu&units=imperial
    Processing Record 41 of Set 1 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?q=cherskiy,ru&units=imperial
    Processing Record 42 of Set 1 | rikitea
    http://api.openweathermap.org/data/2.5/weather?q=rikitea,pf&units=imperial
    Processing Record 43 of Set 1 | vaini
    http://api.openweathermap.org/data/2.5/weather?q=vaini,to&units=imperial
    Processing Record 44 of Set 1 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?q=new_norfolk,au&units=imperial
    Processing Record 45 of Set 1 | salug
    http://api.openweathermap.org/data/2.5/weather?q=salug,ph&units=imperial
    Processing Record 46 of Set 1 | praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?q=praia_da_vitoria,pt&units=imperial
    Processing Record 47 of Set 1 | denpasar
    http://api.openweathermap.org/data/2.5/weather?q=denpasar,id&units=imperial
    Processing Record 48 of Set 1 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?q=saint-philippe,re&units=imperial
    Processing Record 49 of Set 1 | gewane
    http://api.openweathermap.org/data/2.5/weather?q=gewane,et&units=imperial
    Processing Record 50 of Set 1 | kyabe
    http://api.openweathermap.org/data/2.5/weather?q=kyabe,td&units=imperial
    Processing Record 1 of Set 2 | fukue
    http://api.openweathermap.org/data/2.5/weather?q=fukue,jp&units=imperial
    Processing Record 2 of Set 2 | bijni
    http://api.openweathermap.org/data/2.5/weather?q=bijni,in&units=imperial
    Processing Record 3 of Set 2 | acarau
    http://api.openweathermap.org/data/2.5/weather?q=acarau,br&units=imperial
    Processing Record 4 of Set 2 | port-gentil
    http://api.openweathermap.org/data/2.5/weather?q=port-gentil,ga&units=imperial
    Processing Record 5 of Set 2 | port alfred
    http://api.openweathermap.org/data/2.5/weather?q=port_alfred,za&units=imperial
    Processing Record 6 of Set 2 | naze
    http://api.openweathermap.org/data/2.5/weather?q=naze,jp&units=imperial
    Processing Record 7 of Set 2 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?q=bengkulu,id&units=imperial
    Processing Record 8 of Set 2 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?q=sao_filipe,cv&units=imperial
    Processing Record 9 of Set 2 | kosh-agach
    http://api.openweathermap.org/data/2.5/weather?q=kosh-agach,ru&units=imperial
    Processing Record 10 of Set 2 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?q=longyearbyen,sj&units=imperial
    Processing Record 11 of Set 2 | kang
    http://api.openweathermap.org/data/2.5/weather?q=kang,bw&units=imperial
    Processing Record 12 of Set 2 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?q=chokurdakh,ru&units=imperial
    Processing Record 13 of Set 2 | kulhudhuffushi
    http://api.openweathermap.org/data/2.5/weather?q=kulhudhuffushi,mv&units=imperial
    Processing Record 14 of Set 2 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?q=arraial_do_cabo,br&units=imperial
    Processing Record 15 of Set 2 | independence
    http://api.openweathermap.org/data/2.5/weather?q=independence,us&units=imperial
    Processing Record 16 of Set 2 | souillac
    http://api.openweathermap.org/data/2.5/weather?q=souillac,mu&units=imperial
    Processing Record 17 of Set 2 | sorland
    http://api.openweathermap.org/data/2.5/weather?q=sorland,no&units=imperial
    Processing Record 18 of Set 2 | manzhouli
    http://api.openweathermap.org/data/2.5/weather?q=manzhouli,cn&units=imperial
    Processing Record 19 of Set 2 | svetlogorsk
    http://api.openweathermap.org/data/2.5/weather?q=svetlogorsk,ru&units=imperial
    Processing Record 20 of Set 2 | mount isa
    http://api.openweathermap.org/data/2.5/weather?q=mount_isa,au&units=imperial
    Processing Record 21 of Set 2 | chakwal
    http://api.openweathermap.org/data/2.5/weather?q=chakwal,pk&units=imperial
    Processing Record 22 of Set 2 | grand gaube
    http://api.openweathermap.org/data/2.5/weather?q=grand_gaube,mu&units=imperial
    Processing Record 23 of Set 2 | havre-saint-pierre
    http://api.openweathermap.org/data/2.5/weather?q=havre-saint-pierre,ca&units=imperial
    Processing Record 24 of Set 2 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?q=hithadhoo,mv&units=imperial
    Processing Record 25 of Set 2 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?q=guerrero_negro,mx&units=imperial
    Processing Record 26 of Set 2 | penaranda
    http://api.openweathermap.org/data/2.5/weather?q=penaranda,ph&units=imperial
    Processing Record 27 of Set 2 | nikolsk
    http://api.openweathermap.org/data/2.5/weather?q=nikolsk,ru&units=imperial
    Processing Record 28 of Set 2 | iralaya
    http://api.openweathermap.org/data/2.5/weather?q=iralaya,hn&units=imperial
    Processing Record 29 of Set 2 | dongsheng
    http://api.openweathermap.org/data/2.5/weather?q=dongsheng,cn&units=imperial
    Processing Record 30 of Set 2 | poum
    http://api.openweathermap.org/data/2.5/weather?q=poum,nc&units=imperial
    Processing Record 31 of Set 2 | cape town
    http://api.openweathermap.org/data/2.5/weather?q=cape_town,za&units=imperial
    Processing Record 32 of Set 2 | hualmay
    http://api.openweathermap.org/data/2.5/weather?q=hualmay,pe&units=imperial
    Processing Record 33 of Set 2 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?q=leningradskiy,ru&units=imperial
    Processing Record 34 of Set 2 | fortuna
    http://api.openweathermap.org/data/2.5/weather?q=fortuna,us&units=imperial
    Processing Record 35 of Set 2 | cerrito
    http://api.openweathermap.org/data/2.5/weather?q=cerrito,py&units=imperial
    Processing Record 36 of Set 2 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?q=komsomolskiy,ru&units=imperial
    Processing Record 37 of Set 2 | linxia
    http://api.openweathermap.org/data/2.5/weather?q=linxia,cn&units=imperial
    Processing Record 38 of Set 2 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?q=port_elizabeth,za&units=imperial
    Processing Record 39 of Set 2 | victoria
    http://api.openweathermap.org/data/2.5/weather?q=victoria,sc&units=imperial
    Processing Record 40 of Set 2 | port blair
    http://api.openweathermap.org/data/2.5/weather?q=port_blair,in&units=imperial
    Processing Record 41 of Set 2 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?q=nanortalik,gl&units=imperial
    Processing Record 42 of Set 2 | abashiri
    http://api.openweathermap.org/data/2.5/weather?q=abashiri,jp&units=imperial
    Processing Record 43 of Set 2 | praia
    http://api.openweathermap.org/data/2.5/weather?q=praia,cv&units=imperial
    Processing Record 44 of Set 2 | barrow
    http://api.openweathermap.org/data/2.5/weather?q=barrow,us&units=imperial
    Processing Record 45 of Set 2 | aljezur
    http://api.openweathermap.org/data/2.5/weather?q=aljezur,pt&units=imperial
    Processing Record 46 of Set 2 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?q=saskylakh,ru&units=imperial
    Processing Record 47 of Set 2 | lebu
    http://api.openweathermap.org/data/2.5/weather?q=lebu,cl&units=imperial
    Processing Record 48 of Set 2 | kamaishi
    http://api.openweathermap.org/data/2.5/weather?q=kamaishi,jp&units=imperial
    Processing Record 49 of Set 2 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?q=punta_arenas,cl&units=imperial
    Processing Record 50 of Set 2 | rauma
    http://api.openweathermap.org/data/2.5/weather?q=rauma,fi&units=imperial
    Processing Record 1 of Set 3 | necochea
    http://api.openweathermap.org/data/2.5/weather?q=necochea,ar&units=imperial
    Processing Record 2 of Set 3 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?q=puerto_ayora,ec&units=imperial
    Processing Record 3 of Set 3 | santa isabel do rio negro
    http://api.openweathermap.org/data/2.5/weather?q=santa_isabel_do_rio_negro,br&units=imperial
    Processing Record 4 of Set 3 | hilo
    http://api.openweathermap.org/data/2.5/weather?q=hilo,us&units=imperial
    Processing Record 5 of Set 3 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?q=bredasdorp,za&units=imperial
    Processing Record 6 of Set 3 | cullen
    http://api.openweathermap.org/data/2.5/weather?q=cullen,gb&units=imperial
    Processing Record 7 of Set 3 | fochville
    http://api.openweathermap.org/data/2.5/weather?q=fochville,za&units=imperial
    Processing Record 8 of Set 3 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?q=kruisfontein,za&units=imperial
    Processing Record 9 of Set 3 | dunedin
    http://api.openweathermap.org/data/2.5/weather?q=dunedin,nz&units=imperial
    Processing Record 10 of Set 3 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?q=qaanaaq,gl&units=imperial
    Processing Record 11 of Set 3 | hermanus
    http://api.openweathermap.org/data/2.5/weather?q=hermanus,za&units=imperial
    Processing Record 12 of Set 3 | bethel
    http://api.openweathermap.org/data/2.5/weather?q=bethel,us&units=imperial
    Processing Record 13 of Set 3 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?q=pacific_grove,us&units=imperial
    Processing Record 14 of Set 3 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?q=pangnirtung,ca&units=imperial
    Processing Record 15 of Set 3 | avarua
    http://api.openweathermap.org/data/2.5/weather?q=avarua,ck&units=imperial
    Processing Record 16 of Set 3 | palu
    http://api.openweathermap.org/data/2.5/weather?q=palu,id&units=imperial
    Processing Record 17 of Set 3 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?q=nouadhibou,mr&units=imperial
    Processing Record 18 of Set 3 | hobart
    http://api.openweathermap.org/data/2.5/weather?q=hobart,au&units=imperial
    Processing Record 19 of Set 3 | port keats
    http://api.openweathermap.org/data/2.5/weather?q=port_keats,au&units=imperial
    Processing Record 20 of Set 3 | korla
    http://api.openweathermap.org/data/2.5/weather?q=korla,cn&units=imperial
    Processing Record 21 of Set 3 | ambilobe
    http://api.openweathermap.org/data/2.5/weather?q=ambilobe,mg&units=imperial
    Processing Record 22 of Set 3 | mnogovershinnyy
    http://api.openweathermap.org/data/2.5/weather?q=mnogovershinnyy,ru&units=imperial
    Processing Record 23 of Set 3 | adelaide
    http://api.openweathermap.org/data/2.5/weather?q=adelaide,au&units=imperial
    Processing Record 24 of Set 3 | ancud
    http://api.openweathermap.org/data/2.5/weather?q=ancud,cl&units=imperial
    Processing Record 25 of Set 3 | khatanga
    http://api.openweathermap.org/data/2.5/weather?q=khatanga,ru&units=imperial
    Processing Record 26 of Set 3 | saint george
    http://api.openweathermap.org/data/2.5/weather?q=saint_george,bm&units=imperial
    Processing Record 27 of Set 3 | abomey
    http://api.openweathermap.org/data/2.5/weather?q=abomey,bj&units=imperial
    Processing Record 28 of Set 3 | modasa
    http://api.openweathermap.org/data/2.5/weather?q=modasa,in&units=imperial
    Processing Record 29 of Set 3 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?q=meulaboh,id&units=imperial
    Processing Record 30 of Set 3 | bilibino
    http://api.openweathermap.org/data/2.5/weather?q=bilibino,ru&units=imperial
    Processing Record 31 of Set 3 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?q=ribeira_grande,pt&units=imperial
    Processing Record 32 of Set 3 | sitka
    http://api.openweathermap.org/data/2.5/weather?q=sitka,us&units=imperial
    Processing Record 33 of Set 3 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?q=tuatapere,nz&units=imperial
    Processing Record 34 of Set 3 | berlevag
    http://api.openweathermap.org/data/2.5/weather?q=berlevag,no&units=imperial
    Processing Record 35 of Set 3 | yumen
    http://api.openweathermap.org/data/2.5/weather?q=yumen,cn&units=imperial
    Processing Record 36 of Set 3 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?q=tasiilaq,gl&units=imperial
    Processing Record 37 of Set 3 | margherita
    http://api.openweathermap.org/data/2.5/weather?q=margherita,in&units=imperial
    Processing Record 38 of Set 3 | chato
    http://api.openweathermap.org/data/2.5/weather?q=chato,tz&units=imperial
    Processing Record 39 of Set 3 | talnakh
    http://api.openweathermap.org/data/2.5/weather?q=talnakh,ru&units=imperial
    Processing Record 40 of Set 3 | lompoc
    http://api.openweathermap.org/data/2.5/weather?q=lompoc,us&units=imperial
    Processing Record 41 of Set 3 | malanje
    http://api.openweathermap.org/data/2.5/weather?q=malanje,ao&units=imperial
    Processing Record 42 of Set 3 | bluff
    http://api.openweathermap.org/data/2.5/weather?q=bluff,nz&units=imperial
    Processing Record 43 of Set 3 | tiksi
    http://api.openweathermap.org/data/2.5/weather?q=tiksi,ru&units=imperial
    Processing Record 44 of Set 3 | saldanha
    http://api.openweathermap.org/data/2.5/weather?q=saldanha,za&units=imperial
    Processing Record 45 of Set 3 | cacequi
    http://api.openweathermap.org/data/2.5/weather?q=cacequi,br&units=imperial
    Processing Record 46 of Set 3 | tome
    http://api.openweathermap.org/data/2.5/weather?q=tome,cl&units=imperial
    Processing Record 47 of Set 3 | svetlyy
    http://api.openweathermap.org/data/2.5/weather?q=svetlyy,ru&units=imperial
    Processing Record 48 of Set 3 | santa maria
    http://api.openweathermap.org/data/2.5/weather?q=santa_maria,cv&units=imperial
    Processing Record 49 of Set 3 | karratha
    http://api.openweathermap.org/data/2.5/weather?q=karratha,au&units=imperial
    Processing Record 50 of Set 3 | lokhvytsya
    http://api.openweathermap.org/data/2.5/weather?q=lokhvytsya,ua&units=imperial
    Processing Record 1 of Set 4 | bali chak
    http://api.openweathermap.org/data/2.5/weather?q=bali_chak,in&units=imperial
    Processing Record 2 of Set 4 | la spezia
    http://api.openweathermap.org/data/2.5/weather?q=la_spezia,it&units=imperial
    Processing Record 3 of Set 4 | hamilton
    http://api.openweathermap.org/data/2.5/weather?q=hamilton,bm&units=imperial
    Processing Record 4 of Set 4 | borogontsy
    http://api.openweathermap.org/data/2.5/weather?q=borogontsy,ru&units=imperial
    Processing Record 5 of Set 4 | ouesso
    http://api.openweathermap.org/data/2.5/weather?q=ouesso,cg&units=imperial
    Processing Record 6 of Set 4 | felanitx
    http://api.openweathermap.org/data/2.5/weather?q=felanitx,es&units=imperial
    Processing Record 7 of Set 4 | biltine
    http://api.openweathermap.org/data/2.5/weather?q=biltine,td&units=imperial
    Processing Record 8 of Set 4 | xining
    http://api.openweathermap.org/data/2.5/weather?q=xining,cn&units=imperial
    Processing Record 9 of Set 4 | nakamura
    http://api.openweathermap.org/data/2.5/weather?q=nakamura,jp&units=imperial
    Processing Record 10 of Set 4 | vila velha
    http://api.openweathermap.org/data/2.5/weather?q=vila_velha,br&units=imperial
    Processing Record 11 of Set 4 | poronaysk
    http://api.openweathermap.org/data/2.5/weather?q=poronaysk,ru&units=imperial
    Processing Record 12 of Set 4 | westport
    http://api.openweathermap.org/data/2.5/weather?q=westport,ie&units=imperial
    Processing Record 13 of Set 4 | vanimo
    http://api.openweathermap.org/data/2.5/weather?q=vanimo,pg&units=imperial
    Processing Record 14 of Set 4 | sembe
    http://api.openweathermap.org/data/2.5/weather?q=sembe,cg&units=imperial
    Processing Record 15 of Set 4 | pa daet
    http://api.openweathermap.org/data/2.5/weather?q=pa_daet,th&units=imperial
    Processing Record 16 of Set 4 | ponta delgada
    http://api.openweathermap.org/data/2.5/weather?q=ponta_delgada,pt&units=imperial
    Processing Record 17 of Set 4 | baykit
    http://api.openweathermap.org/data/2.5/weather?q=baykit,ru&units=imperial
    Processing Record 18 of Set 4 | jamestown
    http://api.openweathermap.org/data/2.5/weather?q=jamestown,sh&units=imperial
    Processing Record 19 of Set 4 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?q=mar_del_plata,ar&units=imperial
    Processing Record 20 of Set 4 | evensk
    http://api.openweathermap.org/data/2.5/weather?q=evensk,ru&units=imperial
    Processing Record 21 of Set 4 | santa cruz de la palma
    http://api.openweathermap.org/data/2.5/weather?q=santa_cruz_de_la_palma,es&units=imperial
    Processing Record 22 of Set 4 | batemans bay
    http://api.openweathermap.org/data/2.5/weather?q=batemans_bay,au&units=imperial
    Processing Record 23 of Set 4 | fatehpur
    http://api.openweathermap.org/data/2.5/weather?q=fatehpur,in&units=imperial
    Processing Record 24 of Set 4 | chuy
    http://api.openweathermap.org/data/2.5/weather?q=chuy,uy&units=imperial
    Processing Record 25 of Set 4 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?q=ilulissat,gl&units=imperial
    Processing Record 26 of Set 4 | mitchell
    http://api.openweathermap.org/data/2.5/weather?q=mitchell,us&units=imperial
    Processing Record 27 of Set 4 | carutapera
    http://api.openweathermap.org/data/2.5/weather?q=carutapera,br&units=imperial
    Processing Record 28 of Set 4 | roros
    http://api.openweathermap.org/data/2.5/weather?q=roros,no&units=imperial
    Processing Record 29 of Set 4 | havelock
    http://api.openweathermap.org/data/2.5/weather?q=havelock,us&units=imperial
    Processing Record 30 of Set 4 | manicore
    http://api.openweathermap.org/data/2.5/weather?q=manicore,br&units=imperial
    Processing Record 31 of Set 4 | koryazhma
    http://api.openweathermap.org/data/2.5/weather?q=koryazhma,ru&units=imperial
    Processing Record 32 of Set 4 | oshikango
    http://api.openweathermap.org/data/2.5/weather?q=oshikango,na&units=imperial
    Processing Record 33 of Set 4 | caravelas
    http://api.openweathermap.org/data/2.5/weather?q=caravelas,br&units=imperial
    Processing Record 34 of Set 4 | geraldton
    http://api.openweathermap.org/data/2.5/weather?q=geraldton,au&units=imperial
    Processing Record 35 of Set 4 | ahipara
    http://api.openweathermap.org/data/2.5/weather?q=ahipara,nz&units=imperial
    Processing Record 36 of Set 4 | nykoping
    http://api.openweathermap.org/data/2.5/weather?q=nykoping,se&units=imperial
    Processing Record 37 of Set 4 | nileshwar
    http://api.openweathermap.org/data/2.5/weather?q=nileshwar,in&units=imperial
    Processing Record 38 of Set 4 | pevek
    http://api.openweathermap.org/data/2.5/weather?q=pevek,ru&units=imperial
    Processing Record 39 of Set 4 | beyneu
    http://api.openweathermap.org/data/2.5/weather?q=beyneu,kz&units=imperial
    Processing Record 40 of Set 4 | luderitz
    http://api.openweathermap.org/data/2.5/weather?q=luderitz,na&units=imperial
    Processing Record 41 of Set 4 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?q=severo-kurilsk,ru&units=imperial
    Processing Record 42 of Set 4 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?q=cabo_san_lucas,mx&units=imperial
    Processing Record 43 of Set 4 | norman wells
    http://api.openweathermap.org/data/2.5/weather?q=norman_wells,ca&units=imperial
    Processing Record 44 of Set 4 | araouane
    http://api.openweathermap.org/data/2.5/weather?q=araouane,ml&units=imperial
    Processing Record 45 of Set 4 | broome
    http://api.openweathermap.org/data/2.5/weather?q=broome,au&units=imperial
    Processing Record 46 of Set 4 | oranjestad
    http://api.openweathermap.org/data/2.5/weather?q=oranjestad,aw&units=imperial
    Processing Record 47 of Set 4 | la peca
    http://api.openweathermap.org/data/2.5/weather?q=la_peca,pe&units=imperial
    Processing Record 48 of Set 4 | elizabeth city
    http://api.openweathermap.org/data/2.5/weather?q=elizabeth_city,us&units=imperial
    Processing Record 49 of Set 4 | razdolinsk
    http://api.openweathermap.org/data/2.5/weather?q=razdolinsk,ru&units=imperial
    Processing Record 50 of Set 4 | ossora
    http://api.openweathermap.org/data/2.5/weather?q=ossora,ru&units=imperial
    Processing Record 1 of Set 5 | aswan
    http://api.openweathermap.org/data/2.5/weather?q=aswan,eg&units=imperial
    Processing Record 2 of Set 5 | te anau
    http://api.openweathermap.org/data/2.5/weather?q=te_anau,nz&units=imperial
    Processing Record 3 of Set 5 | saint-joseph
    http://api.openweathermap.org/data/2.5/weather?q=saint-joseph,re&units=imperial
    Processing Record 4 of Set 5 | tromso
    http://api.openweathermap.org/data/2.5/weather?q=tromso,no&units=imperial
    Processing Record 5 of Set 5 | shimoda
    http://api.openweathermap.org/data/2.5/weather?q=shimoda,jp&units=imperial
    Processing Record 6 of Set 5 | dingle
    http://api.openweathermap.org/data/2.5/weather?q=dingle,ie&units=imperial
    Processing Record 7 of Set 5 | pringsewu
    http://api.openweathermap.org/data/2.5/weather?q=pringsewu,id&units=imperial
    Processing Record 8 of Set 5 | bhatapara
    http://api.openweathermap.org/data/2.5/weather?q=bhatapara,in&units=imperial
    Processing Record 9 of Set 5 | viedma
    http://api.openweathermap.org/data/2.5/weather?q=viedma,ar&units=imperial
    Processing Record 10 of Set 5 | georgetown
    http://api.openweathermap.org/data/2.5/weather?q=georgetown,sh&units=imperial
    Processing Record 11 of Set 5 | virginia beach
    http://api.openweathermap.org/data/2.5/weather?q=virginia_beach,us&units=imperial
    Processing Record 12 of Set 5 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?q=iqaluit,ca&units=imperial
    Processing Record 13 of Set 5 | north bend
    http://api.openweathermap.org/data/2.5/weather?q=north_bend,us&units=imperial
    Processing Record 14 of Set 5 | karasjok
    http://api.openweathermap.org/data/2.5/weather?q=karasjok,no&units=imperial
    Processing Record 15 of Set 5 | inuvik
    http://api.openweathermap.org/data/2.5/weather?q=inuvik,ca&units=imperial
    Processing Record 16 of Set 5 | valparaiso
    http://api.openweathermap.org/data/2.5/weather?q=valparaiso,cl&units=imperial
    Processing Record 17 of Set 5 | awbari
    http://api.openweathermap.org/data/2.5/weather?q=awbari,ly&units=imperial
    Processing Record 18 of Set 5 | san juan
    http://api.openweathermap.org/data/2.5/weather?q=san_juan,ar&units=imperial
    Processing Record 19 of Set 5 | eregli
    http://api.openweathermap.org/data/2.5/weather?q=eregli,tr&units=imperial
    Processing Record 20 of Set 5 | hasaki
    http://api.openweathermap.org/data/2.5/weather?q=hasaki,jp&units=imperial
    Processing Record 21 of Set 5 | ilhabela
    http://api.openweathermap.org/data/2.5/weather?q=ilhabela,br&units=imperial
    Processing Record 22 of Set 5 | asilah
    http://api.openweathermap.org/data/2.5/weather?q=asilah,ma&units=imperial
    Processing Record 23 of Set 5 | pishin
    http://api.openweathermap.org/data/2.5/weather?q=pishin,pk&units=imperial
    Processing Record 24 of Set 5 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?q=lavrentiya,ru&units=imperial
    Processing Record 25 of Set 5 | saint george
    http://api.openweathermap.org/data/2.5/weather?q=saint_george,us&units=imperial
    Processing Record 26 of Set 5 | alyth
    http://api.openweathermap.org/data/2.5/weather?q=alyth,gb&units=imperial
    Processing Record 27 of Set 5 | yulara
    http://api.openweathermap.org/data/2.5/weather?q=yulara,au&units=imperial
    Processing Record 28 of Set 5 | skopelos
    http://api.openweathermap.org/data/2.5/weather?q=skopelos,gr&units=imperial
    Processing Record 29 of Set 5 | kenai
    http://api.openweathermap.org/data/2.5/weather?q=kenai,us&units=imperial
    Processing Record 30 of Set 5 | manokwari
    http://api.openweathermap.org/data/2.5/weather?q=manokwari,id&units=imperial
    Processing Record 31 of Set 5 | karpathos
    http://api.openweathermap.org/data/2.5/weather?q=karpathos,gr&units=imperial
    Processing Record 32 of Set 5 | pangody
    http://api.openweathermap.org/data/2.5/weather?q=pangody,ru&units=imperial
    Processing Record 33 of Set 5 | buta
    http://api.openweathermap.org/data/2.5/weather?q=buta,cd&units=imperial
    Processing Record 34 of Set 5 | homer
    http://api.openweathermap.org/data/2.5/weather?q=homer,us&units=imperial
    Processing Record 35 of Set 5 | oranjemund
    http://api.openweathermap.org/data/2.5/weather?q=oranjemund,na&units=imperial
    Processing Record 36 of Set 5 | sarkand
    http://api.openweathermap.org/data/2.5/weather?q=sarkand,kz&units=imperial
    Processing Record 37 of Set 5 | qaqortoq
    http://api.openweathermap.org/data/2.5/weather?q=qaqortoq,gl&units=imperial
    Processing Record 38 of Set 5 | dali
    http://api.openweathermap.org/data/2.5/weather?q=dali,cn&units=imperial
    Processing Record 39 of Set 5 | kihei
    http://api.openweathermap.org/data/2.5/weather?q=kihei,us&units=imperial
    Processing Record 40 of Set 5 | donegal
    http://api.openweathermap.org/data/2.5/weather?q=donegal,ie&units=imperial
    Processing Record 41 of Set 5 | ketchikan
    http://api.openweathermap.org/data/2.5/weather?q=ketchikan,us&units=imperial
    Processing Record 42 of Set 5 | rosario do sul
    http://api.openweathermap.org/data/2.5/weather?q=rosario_do_sul,br&units=imperial
    Processing Record 43 of Set 5 | salinopolis
    http://api.openweathermap.org/data/2.5/weather?q=salinopolis,br&units=imperial
    Processing Record 44 of Set 5 | guilin
    http://api.openweathermap.org/data/2.5/weather?q=guilin,cn&units=imperial
    Processing Record 45 of Set 5 | isangel
    http://api.openweathermap.org/data/2.5/weather?q=isangel,vu&units=imperial
    Processing Record 46 of Set 5 | esperance
    http://api.openweathermap.org/data/2.5/weather?q=esperance,au&units=imperial
    Processing Record 47 of Set 5 | richards bay
    http://api.openweathermap.org/data/2.5/weather?q=richards_bay,za&units=imperial
    Processing Record 48 of Set 5 | aklavik
    http://api.openweathermap.org/data/2.5/weather?q=aklavik,ca&units=imperial
    Processing Record 49 of Set 5 | mecca
    http://api.openweathermap.org/data/2.5/weather?q=mecca,sa&units=imperial
    Processing Record 50 of Set 5 | suntar
    http://api.openweathermap.org/data/2.5/weather?q=suntar,ru&units=imperial
    Processing Record 1 of Set 6 | kargasok
    http://api.openweathermap.org/data/2.5/weather?q=kargasok,ru&units=imperial
    Processing Record 2 of Set 6 | chateaubelair
    http://api.openweathermap.org/data/2.5/weather?q=chateaubelair,vc&units=imperial
    Processing Record 3 of Set 6 | davila
    http://api.openweathermap.org/data/2.5/weather?q=davila,ph&units=imperial
    Processing Record 4 of Set 6 | bayevo
    http://api.openweathermap.org/data/2.5/weather?q=bayevo,ru&units=imperial
    Processing Record 5 of Set 6 | katsuura
    http://api.openweathermap.org/data/2.5/weather?q=katsuura,jp&units=imperial
    Processing Record 6 of Set 6 | diamantino
    http://api.openweathermap.org/data/2.5/weather?q=diamantino,br&units=imperial
    Processing Record 7 of Set 6 | ariquemes
    http://api.openweathermap.org/data/2.5/weather?q=ariquemes,br&units=imperial
    Processing Record 8 of Set 6 | katima mulilo
    http://api.openweathermap.org/data/2.5/weather?q=katima_mulilo,na&units=imperial
    Processing Record 9 of Set 6 | puro
    http://api.openweathermap.org/data/2.5/weather?q=puro,ph&units=imperial
    Processing Record 10 of Set 6 | kamenka
    http://api.openweathermap.org/data/2.5/weather?q=kamenka,ru&units=imperial
    Processing Record 11 of Set 6 | zhanaozen
    http://api.openweathermap.org/data/2.5/weather?q=zhanaozen,kz&units=imperial
    Processing Record 12 of Set 6 | alice springs
    http://api.openweathermap.org/data/2.5/weather?q=alice_springs,au&units=imperial
    Processing Record 13 of Set 6 | ochakiv
    http://api.openweathermap.org/data/2.5/weather?q=ochakiv,ua&units=imperial
    Processing Record 14 of Set 6 | kurikka
    http://api.openweathermap.org/data/2.5/weather?q=kurikka,fi&units=imperial
    Processing Record 15 of Set 6 | adeje
    http://api.openweathermap.org/data/2.5/weather?q=adeje,es&units=imperial
    Processing Record 16 of Set 6 | cayenne
    http://api.openweathermap.org/data/2.5/weather?q=cayenne,gf&units=imperial
    Processing Record 17 of Set 6 | gardez
    http://api.openweathermap.org/data/2.5/weather?q=gardez,af&units=imperial
    Processing Record 18 of Set 6 | sandpoint
    http://api.openweathermap.org/data/2.5/weather?q=sandpoint,us&units=imperial
    Processing Record 19 of Set 6 | sawtell
    http://api.openweathermap.org/data/2.5/weather?q=sawtell,au&units=imperial
    Processing Record 20 of Set 6 | zabid
    http://api.openweathermap.org/data/2.5/weather?q=zabid,ye&units=imperial
    Processing Record 21 of Set 6 | sedlcany
    http://api.openweathermap.org/data/2.5/weather?q=sedlcany,cz&units=imperial
    Processing Record 22 of Set 6 | srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?q=srednekolymsk,ru&units=imperial
    Processing Record 23 of Set 6 | beloha
    http://api.openweathermap.org/data/2.5/weather?q=beloha,mg&units=imperial
    Processing Record 24 of Set 6 | paracuru
    http://api.openweathermap.org/data/2.5/weather?q=paracuru,br&units=imperial
    Processing Record 25 of Set 6 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?q=okhotsk,ru&units=imperial
    Processing Record 26 of Set 6 | roma
    http://api.openweathermap.org/data/2.5/weather?q=roma,au&units=imperial
    Processing Record 27 of Set 6 | ahuimanu
    http://api.openweathermap.org/data/2.5/weather?q=ahuimanu,us&units=imperial
    Processing Record 28 of Set 6 | bonavista
    http://api.openweathermap.org/data/2.5/weather?q=bonavista,ca&units=imperial
    Processing Record 29 of Set 6 | nisia floresta
    http://api.openweathermap.org/data/2.5/weather?q=nisia_floresta,br&units=imperial
    Processing Record 30 of Set 6 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?q=coquimbo,cl&units=imperial
    Processing Record 31 of Set 6 | barcelona
    http://api.openweathermap.org/data/2.5/weather?q=barcelona,ph&units=imperial
    Processing Record 32 of Set 6 | aberdeen
    http://api.openweathermap.org/data/2.5/weather?q=aberdeen,us&units=imperial
    Processing Record 33 of Set 6 | warrnambool
    http://api.openweathermap.org/data/2.5/weather?q=warrnambool,au&units=imperial
    Processing Record 34 of Set 6 | scarborough
    http://api.openweathermap.org/data/2.5/weather?q=scarborough,tt&units=imperial
    Processing Record 35 of Set 6 | morgan city
    http://api.openweathermap.org/data/2.5/weather?q=morgan_city,us&units=imperial
    Processing Record 36 of Set 6 | tahta
    http://api.openweathermap.org/data/2.5/weather?q=tahta,eg&units=imperial
    Processing Record 37 of Set 6 | gopalpur
    http://api.openweathermap.org/data/2.5/weather?q=gopalpur,in&units=imperial
    Processing Record 38 of Set 6 | tiznit
    http://api.openweathermap.org/data/2.5/weather?q=tiznit,ma&units=imperial
    Processing Record 39 of Set 6 | laguna
    http://api.openweathermap.org/data/2.5/weather?q=laguna,br&units=imperial
    Processing Record 40 of Set 6 | laguna de perlas
    http://api.openweathermap.org/data/2.5/weather?q=laguna_de_perlas,ni&units=imperial
    Processing Record 41 of Set 6 | dunmore town
    http://api.openweathermap.org/data/2.5/weather?q=dunmore_town,bs&units=imperial
    Processing Record 42 of Set 6 | bagojo
    http://api.openweathermap.org/data/2.5/weather?q=bagojo,mx&units=imperial
    Processing Record 43 of Set 6 | vung tau
    http://api.openweathermap.org/data/2.5/weather?q=vung_tau,vn&units=imperial
    Processing Record 44 of Set 6 | havoysund
    http://api.openweathermap.org/data/2.5/weather?q=havoysund,no&units=imperial
    Processing Record 45 of Set 6 | nachingwea
    http://api.openweathermap.org/data/2.5/weather?q=nachingwea,tz&units=imperial
    Processing Record 46 of Set 6 | luganville
    http://api.openweathermap.org/data/2.5/weather?q=luganville,vu&units=imperial
    Processing Record 47 of Set 6 | krasnoarmeyskoye
    http://api.openweathermap.org/data/2.5/weather?q=krasnoarmeyskoye,ru&units=imperial
    Processing Record 48 of Set 6 | komyshuvakha
    http://api.openweathermap.org/data/2.5/weather?q=komyshuvakha,ua&units=imperial
    Processing Record 49 of Set 6 | luau
    http://api.openweathermap.org/data/2.5/weather?q=luau,ao&units=imperial
    Processing Record 50 of Set 6 | altagracia de orituco
    http://api.openweathermap.org/data/2.5/weather?q=altagracia_de_orituco,ve&units=imperial
    Processing Record 1 of Set 7 | touros
    http://api.openweathermap.org/data/2.5/weather?q=touros,br&units=imperial
    Processing Record 2 of Set 7 | rio gallegos
    http://api.openweathermap.org/data/2.5/weather?q=rio_gallegos,ar&units=imperial
    Processing Record 3 of Set 7 | baruun-urt
    http://api.openweathermap.org/data/2.5/weather?q=baruun-urt,mn&units=imperial
    Processing Record 4 of Set 7 | deputatskiy
    http://api.openweathermap.org/data/2.5/weather?q=deputatskiy,ru&units=imperial
    Processing Record 5 of Set 7 | cidreira
    http://api.openweathermap.org/data/2.5/weather?q=cidreira,br&units=imperial
    Processing Record 6 of Set 7 | hambantota
    http://api.openweathermap.org/data/2.5/weather?q=hambantota,lk&units=imperial
    Processing Record 7 of Set 7 | la ronge
    http://api.openweathermap.org/data/2.5/weather?q=la_ronge,ca&units=imperial
    Processing Record 8 of Set 7 | ust-kuyga
    http://api.openweathermap.org/data/2.5/weather?q=ust-kuyga,ru&units=imperial
    Processing Record 9 of Set 7 | georgetown
    http://api.openweathermap.org/data/2.5/weather?q=georgetown,gy&units=imperial
    Processing Record 10 of Set 7 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?q=fairbanks,us&units=imperial
    Processing Record 11 of Set 7 | abu dhabi
    http://api.openweathermap.org/data/2.5/weather?q=abu_dhabi,ae&units=imperial
    Processing Record 12 of Set 7 | kiama
    http://api.openweathermap.org/data/2.5/weather?q=kiama,au&units=imperial
    Processing Record 13 of Set 7 | pangai
    http://api.openweathermap.org/data/2.5/weather?q=pangai,to&units=imperial
    Processing Record 14 of Set 7 | kibre mengist
    http://api.openweathermap.org/data/2.5/weather?q=kibre_mengist,et&units=imperial
    Processing Record 15 of Set 7 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?q=ponta_do_sol,cv&units=imperial
    Processing Record 16 of Set 7 | adrar
    http://api.openweathermap.org/data/2.5/weather?q=adrar,dz&units=imperial
    Processing Record 17 of Set 7 | sungaipenuh
    http://api.openweathermap.org/data/2.5/weather?q=sungaipenuh,id&units=imperial
    Processing Record 18 of Set 7 | shatki
    http://api.openweathermap.org/data/2.5/weather?q=shatki,ru&units=imperial
    Processing Record 19 of Set 7 | grootfontein
    http://api.openweathermap.org/data/2.5/weather?q=grootfontein,na&units=imperial
    Processing Record 20 of Set 7 | sharya
    http://api.openweathermap.org/data/2.5/weather?q=sharya,ru&units=imperial
    Processing Record 21 of Set 7 | camalu
    http://api.openweathermap.org/data/2.5/weather?q=camalu,mx&units=imperial
    Processing Record 22 of Set 7 | salalah
    http://api.openweathermap.org/data/2.5/weather?q=salalah,om&units=imperial
    Processing Record 23 of Set 7 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?q=mahebourg,mu&units=imperial
    Processing Record 24 of Set 7 | mariestad
    http://api.openweathermap.org/data/2.5/weather?q=mariestad,se&units=imperial
    Processing Record 25 of Set 7 | ostersund
    http://api.openweathermap.org/data/2.5/weather?q=ostersund,se&units=imperial
    Processing Record 26 of Set 7 | yantal
    http://api.openweathermap.org/data/2.5/weather?q=yantal,ru&units=imperial
    Processing Record 27 of Set 7 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?q=ponta_do_sol,pt&units=imperial
    Processing Record 28 of Set 7 | marsh harbour
    http://api.openweathermap.org/data/2.5/weather?q=marsh_harbour,bs&units=imperial
    Processing Record 29 of Set 7 | alyangula
    http://api.openweathermap.org/data/2.5/weather?q=alyangula,au&units=imperial
    Processing Record 30 of Set 7 | rio bueno
    http://api.openweathermap.org/data/2.5/weather?q=rio_bueno,cl&units=imperial
    Processing Record 31 of Set 7 | yuksekova
    http://api.openweathermap.org/data/2.5/weather?q=yuksekova,tr&units=imperial
    Processing Record 32 of Set 7 | miyang
    http://api.openweathermap.org/data/2.5/weather?q=miyang,cn&units=imperial
    Processing Record 33 of Set 7 | grindavik
    http://api.openweathermap.org/data/2.5/weather?q=grindavik,is&units=imperial
    Processing Record 34 of Set 7 | tema
    http://api.openweathermap.org/data/2.5/weather?q=tema,gh&units=imperial
    Processing Record 35 of Set 7 | nome
    http://api.openweathermap.org/data/2.5/weather?q=nome,us&units=imperial
    Processing Record 36 of Set 7 | namatanai
    http://api.openweathermap.org/data/2.5/weather?q=namatanai,pg&units=imperial
    Processing Record 37 of Set 7 | lorengau
    http://api.openweathermap.org/data/2.5/weather?q=lorengau,pg&units=imperial
    Processing Record 38 of Set 7 | upernavik
    http://api.openweathermap.org/data/2.5/weather?q=upernavik,gl&units=imperial
    Processing Record 39 of Set 7 | stoke-on-trent
    http://api.openweathermap.org/data/2.5/weather?q=stoke-on-trent,gb&units=imperial
    Processing Record 40 of Set 7 | nikel
    http://api.openweathermap.org/data/2.5/weather?q=nikel,ru&units=imperial
    Processing Record 41 of Set 7 | lufilufi
    http://api.openweathermap.org/data/2.5/weather?q=lufilufi,ws&units=imperial
    Processing Record 42 of Set 7 | rundu
    http://api.openweathermap.org/data/2.5/weather?q=rundu,na&units=imperial
    Processing Record 43 of Set 7 | barsovo
    http://api.openweathermap.org/data/2.5/weather?q=barsovo,ru&units=imperial
    Processing Record 44 of Set 7 | mrakovo
    http://api.openweathermap.org/data/2.5/weather?q=mrakovo,ru&units=imperial
    Processing Record 45 of Set 7 | castro
    http://api.openweathermap.org/data/2.5/weather?q=castro,cl&units=imperial
    Processing Record 46 of Set 7 | lucapa
    http://api.openweathermap.org/data/2.5/weather?q=lucapa,ao&units=imperial
    Processing Record 47 of Set 7 | kerman
    http://api.openweathermap.org/data/2.5/weather?q=kerman,ir&units=imperial
    Processing Record 48 of Set 7 | dzilam gonzalez
    http://api.openweathermap.org/data/2.5/weather?q=dzilam_gonzalez,mx&units=imperial
    Processing Record 49 of Set 7 | ampanihy
    http://api.openweathermap.org/data/2.5/weather?q=ampanihy,mg&units=imperial
    Processing Record 50 of Set 7 | la rioja
    http://api.openweathermap.org/data/2.5/weather?q=la_rioja,ar&units=imperial
    Processing Record 1 of Set 8 | vostok
    http://api.openweathermap.org/data/2.5/weather?q=vostok,ru&units=imperial
    Processing Record 2 of Set 8 | huarmey
    http://api.openweathermap.org/data/2.5/weather?q=huarmey,pe&units=imperial
    Processing Record 3 of Set 8 | paamiut
    http://api.openweathermap.org/data/2.5/weather?q=paamiut,gl&units=imperial
    Processing Record 4 of Set 8 | sept-iles
    http://api.openweathermap.org/data/2.5/weather?q=sept-iles,ca&units=imperial
    Processing Record 5 of Set 8 | haines junction
    http://api.openweathermap.org/data/2.5/weather?q=haines_junction,ca&units=imperial
    Processing Record 6 of Set 8 | sabzevar
    http://api.openweathermap.org/data/2.5/weather?q=sabzevar,ir&units=imperial
    Processing Record 7 of Set 8 | beaver dam
    http://api.openweathermap.org/data/2.5/weather?q=beaver_dam,us&units=imperial
    Processing Record 8 of Set 8 | bafra
    http://api.openweathermap.org/data/2.5/weather?q=bafra,tr&units=imperial
    Processing Record 9 of Set 8 | mezhdurechenskiy
    http://api.openweathermap.org/data/2.5/weather?q=mezhdurechenskiy,ru&units=imperial
    Processing Record 10 of Set 8 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?q=sisimiut,gl&units=imperial
    Processing Record 11 of Set 8 | ust-ilimsk
    http://api.openweathermap.org/data/2.5/weather?q=ust-ilimsk,ru&units=imperial
    Processing Record 12 of Set 8 | katakwi
    http://api.openweathermap.org/data/2.5/weather?q=katakwi,ug&units=imperial
    Processing Record 13 of Set 8 | ojdula
    http://api.openweathermap.org/data/2.5/weather?q=ojdula,ro&units=imperial
    Processing Record 14 of Set 8 | sistranda
    http://api.openweathermap.org/data/2.5/weather?q=sistranda,no&units=imperial
    Processing Record 15 of Set 8 | puerto madero
    http://api.openweathermap.org/data/2.5/weather?q=puerto_madero,mx&units=imperial
    Processing Record 16 of Set 8 | tres arroyos
    http://api.openweathermap.org/data/2.5/weather?q=tres_arroyos,ar&units=imperial
    Processing Record 17 of Set 8 | portland
    http://api.openweathermap.org/data/2.5/weather?q=portland,au&units=imperial
    Processing Record 18 of Set 8 | mana
    http://api.openweathermap.org/data/2.5/weather?q=mana,gf&units=imperial
    Processing Record 19 of Set 8 | antalaha
    http://api.openweathermap.org/data/2.5/weather?q=antalaha,mg&units=imperial
    Processing Record 20 of Set 8 | thinadhoo
    http://api.openweathermap.org/data/2.5/weather?q=thinadhoo,mv&units=imperial
    Processing Record 21 of Set 8 | bang saphan
    http://api.openweathermap.org/data/2.5/weather?q=bang_saphan,th&units=imperial
    Processing Record 22 of Set 8 | yenagoa
    http://api.openweathermap.org/data/2.5/weather?q=yenagoa,ng&units=imperial
    Processing Record 23 of Set 8 | kapuskasing
    http://api.openweathermap.org/data/2.5/weather?q=kapuskasing,ca&units=imperial
    Processing Record 24 of Set 8 | shenjiamen
    http://api.openweathermap.org/data/2.5/weather?q=shenjiamen,cn&units=imperial
    Processing Record 25 of Set 8 | cardston
    http://api.openweathermap.org/data/2.5/weather?q=cardston,ca&units=imperial
    Processing Record 26 of Set 8 | alta floresta
    http://api.openweathermap.org/data/2.5/weather?q=alta_floresta,br&units=imperial
    Processing Record 27 of Set 8 | neka
    http://api.openweathermap.org/data/2.5/weather?q=neka,ir&units=imperial
    Processing Record 28 of Set 8 | myaundzha
    http://api.openweathermap.org/data/2.5/weather?q=myaundzha,ru&units=imperial
    Processing Record 29 of Set 8 | pisco
    http://api.openweathermap.org/data/2.5/weather?q=pisco,pe&units=imperial
    Processing Record 30 of Set 8 | opuwo
    http://api.openweathermap.org/data/2.5/weather?q=opuwo,na&units=imperial
    Processing Record 31 of Set 8 | yermakovskoye
    http://api.openweathermap.org/data/2.5/weather?q=yermakovskoye,ru&units=imperial
    Processing Record 32 of Set 8 | kabinda
    http://api.openweathermap.org/data/2.5/weather?q=kabinda,cd&units=imperial
    Processing Record 33 of Set 8 | cabot
    http://api.openweathermap.org/data/2.5/weather?q=cabot,us&units=imperial
    Processing Record 34 of Set 8 | fiumicino
    http://api.openweathermap.org/data/2.5/weather?q=fiumicino,it&units=imperial
    Processing Record 35 of Set 8 | tuy hoa
    http://api.openweathermap.org/data/2.5/weather?q=tuy_hoa,vn&units=imperial
    Processing Record 36 of Set 8 | kielce
    http://api.openweathermap.org/data/2.5/weather?q=kielce,pl&units=imperial
    Processing Record 37 of Set 8 | polur
    http://api.openweathermap.org/data/2.5/weather?q=polur,in&units=imperial
    Processing Record 38 of Set 8 | taseyevo
    http://api.openweathermap.org/data/2.5/weather?q=taseyevo,ru&units=imperial
    Processing Record 39 of Set 8 | sola
    http://api.openweathermap.org/data/2.5/weather?q=sola,vu&units=imperial
    Processing Record 40 of Set 8 | amga
    http://api.openweathermap.org/data/2.5/weather?q=amga,ru&units=imperial
    Processing Record 41 of Set 8 | neryungri
    http://api.openweathermap.org/data/2.5/weather?q=neryungri,ru&units=imperial
    Processing Record 42 of Set 8 | ratnagiri
    http://api.openweathermap.org/data/2.5/weather?q=ratnagiri,in&units=imperial
    Processing Record 43 of Set 8 | mahina
    http://api.openweathermap.org/data/2.5/weather?q=mahina,pf&units=imperial
    Processing Record 44 of Set 8 | samarai
    http://api.openweathermap.org/data/2.5/weather?q=samarai,pg&units=imperial
    Processing Record 45 of Set 8 | tadine
    http://api.openweathermap.org/data/2.5/weather?q=tadine,nc&units=imperial
    Processing Record 46 of Set 8 | shahapur
    http://api.openweathermap.org/data/2.5/weather?q=shahapur,in&units=imperial
    Processing Record 47 of Set 8 | aripuana
    http://api.openweathermap.org/data/2.5/weather?q=aripuana,br&units=imperial
    Processing Record 48 of Set 8 | nipawin
    http://api.openweathermap.org/data/2.5/weather?q=nipawin,ca&units=imperial
    Processing Record 49 of Set 8 | maniitsoq
    http://api.openweathermap.org/data/2.5/weather?q=maniitsoq,gl&units=imperial
    Processing Record 50 of Set 8 | scottsbluff
    http://api.openweathermap.org/data/2.5/weather?q=scottsbluff,us&units=imperial
    Processing Record 1 of Set 9 | codajas
    http://api.openweathermap.org/data/2.5/weather?q=codajas,br&units=imperial
    Processing Record 2 of Set 9 | ouadda
    http://api.openweathermap.org/data/2.5/weather?q=ouadda,cf&units=imperial
    Processing Record 3 of Set 9 | colares
    http://api.openweathermap.org/data/2.5/weather?q=colares,pt&units=imperial
    Processing Record 4 of Set 9 | mayo
    http://api.openweathermap.org/data/2.5/weather?q=mayo,ca&units=imperial
    Processing Record 5 of Set 9 | quatre cocos
    http://api.openweathermap.org/data/2.5/weather?q=quatre_cocos,mu&units=imperial
    Processing Record 6 of Set 9 | puerto leguizamo
    http://api.openweathermap.org/data/2.5/weather?q=puerto_leguizamo,co&units=imperial
    Processing Record 7 of Set 9 | macapa
    http://api.openweathermap.org/data/2.5/weather?q=macapa,br&units=imperial
    Processing Record 8 of Set 9 | carauari
    http://api.openweathermap.org/data/2.5/weather?q=carauari,br&units=imperial
    Processing Record 9 of Set 9 | saint-pierre
    http://api.openweathermap.org/data/2.5/weather?q=saint-pierre,pm&units=imperial
    Processing Record 10 of Set 9 | poya
    http://api.openweathermap.org/data/2.5/weather?q=poya,nc&units=imperial
    Processing Record 11 of Set 9 | middleton
    http://api.openweathermap.org/data/2.5/weather?q=middleton,ca&units=imperial
    Processing Record 12 of Set 9 | inyonga
    http://api.openweathermap.org/data/2.5/weather?q=inyonga,tz&units=imperial
    Processing Record 13 of Set 9 | resende
    http://api.openweathermap.org/data/2.5/weather?q=resende,br&units=imperial
    Processing Record 14 of Set 9 | tahe
    http://api.openweathermap.org/data/2.5/weather?q=tahe,cn&units=imperial
    Processing Record 15 of Set 9 | pelym
    http://api.openweathermap.org/data/2.5/weather?q=pelym,ru&units=imperial
    Processing Record 16 of Set 9 | macon
    http://api.openweathermap.org/data/2.5/weather?q=macon,fr&units=imperial
    Processing Record 17 of Set 9 | kalmunai
    http://api.openweathermap.org/data/2.5/weather?q=kalmunai,lk&units=imperial
    Processing Record 18 of Set 9 | mumbwa
    http://api.openweathermap.org/data/2.5/weather?q=mumbwa,zm&units=imperial
    Processing Record 19 of Set 9 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?q=carnarvon,au&units=imperial
    Processing Record 20 of Set 9 | irece
    http://api.openweathermap.org/data/2.5/weather?q=irece,br&units=imperial
    Processing Record 21 of Set 9 | omachi
    http://api.openweathermap.org/data/2.5/weather?q=omachi,jp&units=imperial
    Processing Record 22 of Set 9 | kangaatsiaq
    http://api.openweathermap.org/data/2.5/weather?q=kangaatsiaq,gl&units=imperial
    Processing Record 23 of Set 9 | manhush
    http://api.openweathermap.org/data/2.5/weather?q=manhush,ua&units=imperial
    Processing Record 24 of Set 9 | ciudad valles
    http://api.openweathermap.org/data/2.5/weather?q=ciudad_valles,mx&units=imperial
    Processing Record 25 of Set 9 | roccastrada
    http://api.openweathermap.org/data/2.5/weather?q=roccastrada,it&units=imperial
    Processing Record 26 of Set 9 | vysotsk
    http://api.openweathermap.org/data/2.5/weather?q=vysotsk,ru&units=imperial
    Processing Record 27 of Set 9 | padang
    http://api.openweathermap.org/data/2.5/weather?q=padang,id&units=imperial
    Processing Record 28 of Set 9 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?q=half_moon_bay,us&units=imperial
    Processing Record 29 of Set 9 | orange
    http://api.openweathermap.org/data/2.5/weather?q=orange,au&units=imperial
    Processing Record 30 of Set 9 | auki
    http://api.openweathermap.org/data/2.5/weather?q=auki,sb&units=imperial
    Processing Record 31 of Set 9 | lagoa
    http://api.openweathermap.org/data/2.5/weather?q=lagoa,pt&units=imperial
    Processing Record 32 of Set 9 | saint-leu
    http://api.openweathermap.org/data/2.5/weather?q=saint-leu,re&units=imperial
    Processing Record 33 of Set 9 | andros
    http://api.openweathermap.org/data/2.5/weather?q=andros,gr&units=imperial
    Processing Record 34 of Set 9 | miranda
    http://api.openweathermap.org/data/2.5/weather?q=miranda,br&units=imperial
    Processing Record 35 of Set 9 | ravels
    http://api.openweathermap.org/data/2.5/weather?q=ravels,be&units=imperial
    Processing Record 36 of Set 9 | pacifica
    http://api.openweathermap.org/data/2.5/weather?q=pacifica,us&units=imperial
    Processing Record 37 of Set 9 | the valley
    http://api.openweathermap.org/data/2.5/weather?q=the_valley,ai&units=imperial
    Processing Record 38 of Set 9 | gibara
    http://api.openweathermap.org/data/2.5/weather?q=gibara,cu&units=imperial
    Processing Record 39 of Set 9 | rawson
    http://api.openweathermap.org/data/2.5/weather?q=rawson,ar&units=imperial
    Processing Record 40 of Set 9 | borovoy
    http://api.openweathermap.org/data/2.5/weather?q=borovoy,ru&units=imperial
    Processing Record 41 of Set 9 | akdepe
    http://api.openweathermap.org/data/2.5/weather?q=akdepe,tm&units=imperial
    Processing Record 42 of Set 9 | nasturelu
    http://api.openweathermap.org/data/2.5/weather?q=nasturelu,ro&units=imperial
    Processing Record 43 of Set 9 | key west
    http://api.openweathermap.org/data/2.5/weather?q=key_west,us&units=imperial
    Processing Record 44 of Set 9 | marawi
    http://api.openweathermap.org/data/2.5/weather?q=marawi,sd&units=imperial
    Processing Record 45 of Set 9 | port macquarie
    http://api.openweathermap.org/data/2.5/weather?q=port_macquarie,au&units=imperial
    Processing Record 46 of Set 9 | shingu
    http://api.openweathermap.org/data/2.5/weather?q=shingu,jp&units=imperial
    Processing Record 47 of Set 9 | letnik
    http://api.openweathermap.org/data/2.5/weather?q=letnik,ru&units=imperial
    Processing Record 48 of Set 9 | namibe
    http://api.openweathermap.org/data/2.5/weather?q=namibe,ao&units=imperial
    Processing Record 49 of Set 9 | goya
    http://api.openweathermap.org/data/2.5/weather?q=goya,ar&units=imperial
    Processing Record 50 of Set 9 | barra do garcas
    http://api.openweathermap.org/data/2.5/weather?q=barra_do_garcas,br&units=imperial
    Processing Record 1 of Set 10 | honiara
    http://api.openweathermap.org/data/2.5/weather?q=honiara,sb&units=imperial
    Processing Record 2 of Set 10 | bad hersfeld
    http://api.openweathermap.org/data/2.5/weather?q=bad_hersfeld,de&units=imperial
    Processing Record 3 of Set 10 | quelimane
    http://api.openweathermap.org/data/2.5/weather?q=quelimane,mz&units=imperial
    Processing Record 4 of Set 10 | gagny
    http://api.openweathermap.org/data/2.5/weather?q=gagny,fr&units=imperial
    Processing Record 5 of Set 10 | college
    http://api.openweathermap.org/data/2.5/weather?q=college,us&units=imperial
    Processing Record 6 of Set 10 | kavieng
    http://api.openweathermap.org/data/2.5/weather?q=kavieng,pg&units=imperial
    Processing Record 7 of Set 10 | nizhnevartovsk
    http://api.openweathermap.org/data/2.5/weather?q=nizhnevartovsk,ru&units=imperial
    Processing Record 8 of Set 10 | kutum
    http://api.openweathermap.org/data/2.5/weather?q=kutum,sd&units=imperial
    Processing Record 9 of Set 10 | urengoy
    http://api.openweathermap.org/data/2.5/weather?q=urengoy,ru&units=imperial
    Processing Record 10 of Set 10 | vao
    http://api.openweathermap.org/data/2.5/weather?q=vao,nc&units=imperial
    Processing Record 11 of Set 10 | katsina
    http://api.openweathermap.org/data/2.5/weather?q=katsina,ng&units=imperial
    Processing Record 12 of Set 10 | kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?q=kudahuvadhoo,mv&units=imperial
    Processing Record 13 of Set 10 | kashiwazaki
    http://api.openweathermap.org/data/2.5/weather?q=kashiwazaki,jp&units=imperial
    Processing Record 14 of Set 10 | hayesville
    http://api.openweathermap.org/data/2.5/weather?q=hayesville,us&units=imperial
    Processing Record 15 of Set 10 | basoko
    http://api.openweathermap.org/data/2.5/weather?q=basoko,cd&units=imperial
    Processing Record 16 of Set 10 | ambovombe
    http://api.openweathermap.org/data/2.5/weather?q=ambovombe,mg&units=imperial
    Processing Record 17 of Set 10 | sergeyevka
    http://api.openweathermap.org/data/2.5/weather?q=sergeyevka,kz&units=imperial
    Processing Record 18 of Set 10 | puerto penasco
    http://api.openweathermap.org/data/2.5/weather?q=puerto_penasco,mx&units=imperial
    Processing Record 19 of Set 10 | superior
    http://api.openweathermap.org/data/2.5/weather?q=superior,us&units=imperial
    Processing Record 20 of Set 10 | luangwa
    http://api.openweathermap.org/data/2.5/weather?q=luangwa,zm&units=imperial
    Processing Record 21 of Set 10 | hofn
    http://api.openweathermap.org/data/2.5/weather?q=hofn,is&units=imperial
    Processing Record 22 of Set 10 | tay ninh
    http://api.openweathermap.org/data/2.5/weather?q=tay_ninh,vn&units=imperial
    Processing Record 23 of Set 10 | ixtapa
    http://api.openweathermap.org/data/2.5/weather?q=ixtapa,mx&units=imperial
    Processing Record 24 of Set 10 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?q=san_cristobal,ec&units=imperial
    Processing Record 25 of Set 10 | pontes e lacerda
    http://api.openweathermap.org/data/2.5/weather?q=pontes_e_lacerda,br&units=imperial
    Processing Record 26 of Set 10 | mashpee
    http://api.openweathermap.org/data/2.5/weather?q=mashpee,us&units=imperial
    Processing Record 27 of Set 10 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?q=fort_nelson,ca&units=imperial
    Processing Record 28 of Set 10 | olinda
    http://api.openweathermap.org/data/2.5/weather?q=olinda,br&units=imperial
    Processing Record 29 of Set 10 | penzance
    http://api.openweathermap.org/data/2.5/weather?q=penzance,gb&units=imperial
    Processing Record 30 of Set 10 | mitu
    http://api.openweathermap.org/data/2.5/weather?q=mitu,co&units=imperial
    Processing Record 31 of Set 10 | kahramanmaras
    http://api.openweathermap.org/data/2.5/weather?q=kahramanmaras,tr&units=imperial
    Processing Record 32 of Set 10 | sur
    http://api.openweathermap.org/data/2.5/weather?q=sur,om&units=imperial
    Processing Record 33 of Set 10 | quang ngai
    http://api.openweathermap.org/data/2.5/weather?q=quang_ngai,vn&units=imperial
    Processing Record 34 of Set 10 | gurupa
    http://api.openweathermap.org/data/2.5/weather?q=gurupa,br&units=imperial
    Processing Record 35 of Set 10 | zhangye
    http://api.openweathermap.org/data/2.5/weather?q=zhangye,cn&units=imperial
    Processing Record 36 of Set 10 | narsaq
    http://api.openweathermap.org/data/2.5/weather?q=narsaq,gl&units=imperial
    Processing Record 37 of Set 10 | katherine
    http://api.openweathermap.org/data/2.5/weather?q=katherine,au&units=imperial
    Processing Record 38 of Set 10 | larsnes
    http://api.openweathermap.org/data/2.5/weather?q=larsnes,no&units=imperial
    Processing Record 39 of Set 10 | jardim
    http://api.openweathermap.org/data/2.5/weather?q=jardim,br&units=imperial
    Processing Record 40 of Set 10 | owando
    http://api.openweathermap.org/data/2.5/weather?q=owando,cg&units=imperial
    Processing Record 41 of Set 10 | alipur
    http://api.openweathermap.org/data/2.5/weather?q=alipur,pk&units=imperial
    Processing Record 42 of Set 10 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?q=sinnamary,gf&units=imperial
    Processing Record 43 of Set 10 | panguipulli
    http://api.openweathermap.org/data/2.5/weather?q=panguipulli,cl&units=imperial
    Processing Record 44 of Set 10 | mitsamiouli
    http://api.openweathermap.org/data/2.5/weather?q=mitsamiouli,km&units=imperial
    Processing Record 45 of Set 10 | road town
    http://api.openweathermap.org/data/2.5/weather?q=road_town,vg&units=imperial
    Processing Record 46 of Set 10 | brae
    http://api.openweathermap.org/data/2.5/weather?q=brae,gb&units=imperial
    Processing Record 47 of Set 10 | shannon
    http://api.openweathermap.org/data/2.5/weather?q=shannon,ie&units=imperial
    Processing Record 48 of Set 10 | juquitiba
    http://api.openweathermap.org/data/2.5/weather?q=juquitiba,br&units=imperial
    Processing Record 49 of Set 10 | usak
    http://api.openweathermap.org/data/2.5/weather?q=usak,tr&units=imperial
    Processing Record 50 of Set 10 | makakilo city
    http://api.openweathermap.org/data/2.5/weather?q=makakilo_city,us&units=imperial



```python
city_sample_df = pd.read_csv("City_Data.csv") 

#kept stopping in the middle of editing so saved file, then reopened 
#to ensure I could work on the most recent dataset I'd collected without
#risking freezing my API key from too many calls
#could also just continue with prior dataset city_data_df

city_sample_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Max Temp (F)</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Tashtagol</td>
      <td>80</td>
      <td>RU</td>
      <td>83</td>
      <td>52.77</td>
      <td>87.89</td>
      <td>12.25</td>
      <td>2.73</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Beringovskiy</td>
      <td>64</td>
      <td>RU</td>
      <td>88</td>
      <td>63.05</td>
      <td>179.32</td>
      <td>22.60</td>
      <td>6.08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Challans</td>
      <td>90</td>
      <td>FR</td>
      <td>93</td>
      <td>46.84</td>
      <td>-1.87</td>
      <td>46.40</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Ushuaia</td>
      <td>75</td>
      <td>AR</td>
      <td>41</td>
      <td>-54.80</td>
      <td>-68.30</td>
      <td>60.80</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Sao Joao do Paraiso</td>
      <td>36</td>
      <td>BR</td>
      <td>97</td>
      <td>-15.31</td>
      <td>-42.01</td>
      <td>69.98</td>
      <td>3.62</td>
    </tr>
  </tbody>
</table>
</div>




```python
x_values = city_sample_df["Lat"]
y_values = city_sample_df["Max Temp (F)"]
plt.scatter(x_values,y_values,marker="o", 
            facecolors="blue", edgecolors="black",
            alpha=0.75)
plt.xlim(-90, 90)
plt.ylabel("Maximum Temperature (Fahrenheit)")
plt.xlabel("Latitude")
plt.title(f'City Latitude vs. Max Temperature {now.month}/{now.day}/{now.year}')
plt.savefig("Temperature_v_Latitude.png")

plt.show()
```


![png](output_3_0.png)



```python
x_values = city_sample_df["Lat"]
y_values = city_sample_df["Wind Speed"]
plt.scatter(x_values,y_values,marker="o", 
            facecolors="blue", edgecolors="black",
            alpha=0.75)
plt.xlim(-90, 90)
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.title(f'City Latitude vs. Wind Speed {now.month}/{now.day}/{now.year}')
plt.savefig("WindSpeed_v_Latitude.png")

plt.show()
```


![png](output_4_0.png)



```python
x_values = city_sample_df["Lat"]
y_values = city_sample_df["Humidity"]
plt.scatter(x_values,y_values,marker="o", 
            facecolors="blue", edgecolors="black",
            alpha=0.75)
plt.ylim(0,110)
plt.xlim(-90, 90)
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.title(f'City Latitude vs. Humidity {now.month}/{now.day}/{now.year}')
plt.savefig("Humidity_v_Latitude.png")

plt.show()
```


![png](output_5_0.png)



```python
x_values = city_sample_df["Lat"]
y_values = city_sample_df["Cloudiness"]
plt.scatter(x_values,y_values,marker="o", 
            facecolors="blue", edgecolors="black",
            alpha=0.75)
plt.ylim(0,110)
plt.xlim(-90, 90)
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.title(f'City Latitude vs. Cloudiness {now.month}/{now.day}/{now.year}')
plt.savefig("Cloudiness_v_Latitude.png")

plt.show()
```


![png](output_6_0.png)



```python

```
