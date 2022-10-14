# DADS5001-MiniProject

Topic : วิเคราะห์ความคุ้มค่าในการซื้อคอนโดใน กทม.

Dataset : ข้อมูลอสังหาแต่ละโครงการในประเทศไทยโดยแสดง ที่ตั้ง ราคา พื้นที่และค่าเช่า เป็นต้น โดยเก็บรวบรวมข้อมูลจาก baania ซึ่งเป็นเว็ปไซต์ประกาศขายและเช่าอสังหาริมทรัพย์

Question & Answer : 
- ถ้าต้องการซื้อคอนโดเพื่อปล่อยเช่าที่ไหนที่ผลตอบแทนสูงที่สุด?

Challenge : 
- ข้อมูลราคาค่าเช่าคอนโดที่ได้เป็นค่า median แยกตามเขต ซึ่งทำให้วิเคราะห์ผลตอบแทนจากค่าเช่ารายโครงการไม่ได้ต้องแยกตามเขตแทน
- ข้อมูลราคาค่าเช่าคอนโดมีไม่ครบทุกเขตใน กทม. ซึ่งเกิดจากบางเขตไม่มีคนลงประกาศเช่าใน baania จึงไม่มีข้อมูล
- แต่ละโครงการมีห้องหลายราคาทำให้ยากต่อการเลือกข้อมูลมาใช้ เพราะ ถ้าเลือกใช้ทุกราคาจะทำให้ค่า mean เพี้ยนเนื่องจากจำนวนที่มากเกินไป เนื่องจากหัวข้อเป็นวิเคราะห์ความคุ้มค่าของการซื้อคอนโด จึงเลือกใช้ห้องราคาต่ำที่สุดของแต่ละโครงการและตัดข้อมูลห้องที่เหลือในโครงการทิ้งไป

# Importing Datasets
```
unit = pd.read_csv('opendata_unittype.csv')
proj = pd.read_csv('opendata_project.csv')
liv_sc = pd.read_csv('opendata_living_score.csv')
eat_sc = pd.read_csv('opendata_eating_score.csv')
rent_p = pd.read_csv('opendata_median_price_rent.csv')
```

# Final Dataset
```python
<class 'pandas.core.frame.DataFrame'>
Int64Index: 2368 entries, 0 to 2367
Data columns (total 21 columns):
 #   Column                    Non-Null Count  Dtype         
---  ------                    --------------  -----         
 0   project_id                2368 non-null   object        
 1   subdistrict_id            2368 non-null   float64       
 2   name_en                   2368 non-null   object        
 3   latitude                  2368 non-null   float64       
 4   longitude                 2368 non-null   float64       
 5   subdistrict_name_en       2368 non-null   object        
 6   subdistrict_name_th       2368 non-null   object        
 7   district_name_en          2368 non-null   object        
 8   district_name_th          2368 non-null   object        
 9   province_name_en          2368 non-null   object        
 10  province_name_th          2368 non-null   object        
 11  area_usable_min           2368 non-null   float64       
 12  price_min                 2368 non-null   float64       
 13  date_updated              2368 non-null   datetime64[ns]
 14  pricesqm                  2368 non-null   float64       
 15  eating_price_score        2368 non-null   float64       
 16  eating_quality_score      2368 non-null   float64       
 17  living_score              2368 non-null   float64       
 18  listing_district_name_en  2204 non-null   object        
 19  total_listing             2204 non-null   float64       
 20  median_rent_price_sqm     2204 non-null   float64       
dtypes: datetime64[ns](1), float64(11), object(9)
memory usage: 407.0+ KB
```

# EDA
![image](https://user-images.githubusercontent.com/77285026/195874959-d6e969de-acef-46ba-b311-1177e31af10c.png)

![image](https://user-images.githubusercontent.com/77285026/195882672-0d4b4fe7-d39c-4384-b04e-8d99f2e6acd8.png)

# Q&A
### Q1: ถ้าต้องการซื้อคอนโดเพื่อปล่อยเช่าที่ไหนที่ผลตอบแทนสูงที่สุด?
![image](https://user-images.githubusercontent.com/77285026/195891704-5a5bd723-8316-440a-858e-bc286f89ebff.png)
![image](https://user-images.githubusercontent.com/77285026/195891800-39cfbdb0-d8dd-4ac6-82da-f2636868021f.png)

![image](https://user-images.githubusercontent.com/77285026/195891385-80d5e168-308d-4c96-9421-5923398aec37.png)
### A1: บางกะปิ ลาดพร้าว ลาดกระบัง ผลตอบแทน 9.21% 9.20% 8.70% ตามลำดับ

### Q2: ราคาคอนโดต่อตารางเมตรของแต่ละเขตในกรุงเทพมีความสัมพันธ์กับความสะดวกในการเดินทางและอาหารการกินไหม
เทียบจากคะแนน 
- Living Score คะแนนด้านความสะดวกในการเดินทาง ยิ่งคะแนนสูงความสะดวกในการเดินทางยิ่งมาก
- Eating Quality Score คะแนนด้านคุณภาพอาหาร ยิ่งคะแนนสูงคุณภาพอาหารยิ่งมาก
- Eating Price Score คะแนนด้านราคาอาหาร ยิ่งคะแนนสูงราคาอาหารยิ่งแพง

![image](https://user-images.githubusercontent.com/77285026/195892206-50cc423a-db7b-4ed1-af8f-fa54dc5eb2e2.png)
เทียบ Living Score กับ Condo Price Per SQM โดยสีกำหนดโดยสัดส่วน Living Score ที่ได้ต่อ Condo Price Per SQM
พบว่า Living Score ไปในทางเดียวกับ Condo Price Per SQM แต่เมื่อเทียบสัดส่วนพบว่าราคาต่ำจะมีสัดส่วนที่สูงกว่าราคาสูง สรุปได้ว่าทั้งสองค่าไปในทิศทางเดียวกันแต่เมื่อราคาสูงถึงจุดหนึ่งอาจได้ประโยชน์จากส่วนอื่นที่เพิ่มขึ้นมาแทน

![image](https://user-images.githubusercontent.com/77285026/195892268-416a0c44-df1f-4241-9e89-4586acbd7503.png)

### A2: เขตที่ราคาคอนโดต่อตารางเมตรสูงยิ่งสะดวกในการเดินทางและคุณภาพอาหารที่ดีแต่ราคาอาหารจะแพง เทียบกับเขตที่ราคาคอนโดต่อตารางเมตรต่ำความสะดวกในการเดินทางและคุณภาพอาหารจะน้อยลงแต่ราคาอาหารจะถูกกว่า

# Data Source
## ข้อมูลอสังหา
### Bestimate
- https://gobestimate.com/data-detail/Residental-Project-Data
- https://gobestimate.com/data-detail/Residental-Unittype-Data
- https://gobestimate.com/data-detail/Living-Score-By-Location
- https://gobestimate.com/data-detail/Eating-Score-By-Location
- https://gobestimate.com/data-detail/Median-Rental-Price-by-Location
## ข้อมูลแผนที่
### Bangkok GIS
- http://www.bangkokgis.com/modules.php?m=download_shapefile