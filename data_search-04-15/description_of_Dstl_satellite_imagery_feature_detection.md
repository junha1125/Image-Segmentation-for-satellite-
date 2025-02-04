# Dstl Satellite Imagery Feature Detection

링크 :  https://www.kaggle.com/c/dstl-satellite-imagery-feature-detection/overview 

### 1. overview

위성 이미지의 확산으로 지구에 대한 이해가 획기적으로 개선되었다. 특히, 재난시 자원 동원에서 지구 온난화의 영향 모니터링에 이르기까지 모든 것을보다 잘 달성 할 수있었다.

하지만 이와 같은 발전을 가능케 라벨링 기능을 수작업으로 하거나 불완정한 자동화 시스템에 의존하는 것을 당연시 한다는 것은 문제다.

따라서 크고 복잡한 dataset들이 기하급수적으로 늘어나고 있는 현재, 이미지 분석가들의 수고를 덜어주기 위한 새로운 솔루션을 찾기 위해 대회를 열었다.

 즉, feature labeling을 자동화 하는 방법에 대한 대회이다.



### 2. 데이터에 대한 설명

이 경쟁에서 Dstl은 3-band 및 16-band 형식의 1km x 1km 위성 이미지를 제공

이 대회의 목표는 이 지역에서 발견되는 물체의 class를 감지하고 분류하는 것 



#### 3 밴드 및 16 밴드 이미지

이 경쟁에는 두 가지 유형의 이미지 스펙트럼 컨텐츠가 제공됨

**(1) 3-band 이미지** : 전통적인 RGB 자연 색상 이미지

**(2) 16-band 이미지** : 더 넓은 파장 채널을 캡처하여 스펙트럼 정보가 포함된다. 이러한 다중 대역 이미지는 다중 스펙트럼 (400 – 1040nm) 및 단파 적외선 (SWIR) (1195-2365nm) 범위에서 가져온다. 모든 이미지는 GeoTiff 형식이며 GeoTiff 뷰어 (예 : [QGIS](http://www.qgis.org/) ) 가 필요할 수 있다. 프로그래밍 방식으로 이미지를 보는 방법 에 대한 [자습서](https://www.kaggle.com/c/dstl-satellite-imagery-feature-detection/details/data-processing-tutorial) 를 참조하면 된다.

#### ![직교 준비 표준 이미지 © 2016 DigitalGlobe, Inc.](https://storage.googleapis.com/kaggle-competitions/kaggle/5916/media/6100-Frag_3.png)

모든 이미지 크레디트 : Satellite Imagery © DigitalGlobe, Inc.

#### 이미지 세부 사항

- 센서 : WorldView 3

- Wavebands :

  - Panchromatic : 450-800 nm

  - 8 Multispectral : (빨강, 빨강 가장자리, 해안, 파랑, 녹색, 노랑, 근적외선 및 근적외선) 400 nm-1040 nm
  - 8 SWIR : 1195nm-2365nm

- Nadir에서 센서 해상도 (GSD) :

  - Panchromatic : 0.31m 

  - Multispectral : 1.24 m
  - SWIR : 7.5m로 전달

- Dynamic Range

  - Panchromatic 및 Multispectral : 픽셀 당 11 비트

  - SWIR : 픽셀 당 14 비트

#### 객체 유형

위성 이미지에는 도로, 건물, 차량, 농장, 나무, 수로 등과 같은 다양한 객체가 있음

Dstl은 10 가지 클래스로 분류

1. **건물(Buildings**) -대형 건물, 주거용, 비주거용, 연료 저장 시설, 강화 건물
2. **기타 인공 구조물(Misc. Manmade structures) **
3. **도로(Road)** 
4. **트랙(Track)** -불량 / 흙 / 장바구니, 보도 / 트레일
5. **나무(Trees)** -삼림 지대, 산 울타리, 나무 그룹, 독립형 나무
6. **작물(Crops)** -윤곽 쟁기 / 농지, 곡물 (밀) 작물, 줄 (감자, 순무) 작물
7. **수로(Waterway)** 
8. **서있는 물(Standing water)**
9. **차량 대형(Vehicle Large)** -대형 차량 (예 : 트럭, 트럭, 버스), 물류 차량
10. **차량 소형(Vehicle Small)** -소형 차량 (자동차, 밴), 오토바이

모든 객체 클래스는 단순히 다각형 목록 인 [Polygons](https://en.wikipedia.org/wiki/Polygon) 및 MultiPolygons 의 형태로 설명된다. 이러한 형태에 대해 [GeoJson](http://geojson.org/) 과 [WKT의](https://en.wikipedia.org/wiki/Well-known_text) 두 가지 형식을 제공하며, 이들은 지리 공간 형태를위한 오픈 소스 형식이다. 

제출물은 WKT 형식이다. 

#### 지리 좌표(Geo Coordinates)

제공하는이 데이터 세트에서 x = [0,1] 및 y = [-1,0] 범위의 지리 좌표 세트를 만든다. 이 좌표는 위성 이미지가 촬영 된 위치를 가리기 위해 변환된다. 이미지는 지구상의 같은 지역에서 나왔다.

이러한 이미지를 활용하기 위해 각 이미지의 격자 좌표를 제공하여 이미지의 크기를 조정하고 이미지를 픽셀 단위로 정렬하는 방법을 알 수 있다. 스케일링을 수행하려면 각 이미지에 대해 Xmax 및 Ymin이 필요하다 ( **grid_sizes.csv** 제공 ) 이미지를 프로그래밍 방식으로 보는 방법에 대한 [자습서](https://www.kaggle.com/c/dstl-satellite-imagery-feature-detection/details/data-processing-tutorial) 를 참조

## 파일 설명

- **train_wkt.csv-** 모든 교육 레이블의 WKT 형식

- ImageId-이미지의 ID

- ClassType-객체 유형 (1-10)
- MultipolygonWKT-레이블이 지정된 영역으로 WKT 형식으로 표현 된 다중 다각형 지오메트리 

- **three_band.zip** -3 밴드 위성 이미지의 완전한 데이터 세트. 세 개의 밴드는 파일 이름이 {ImageId} .tif 인 이미지에 있다. MD5 = 7cf7bf17ba3fa3198a401ef67f4ef9b4 

- **sixteen_band.zip** -16 밴드 위성 이미지의 완전한 데이터 세트. 16 개의 밴드는 파일 이름이 {ImageId} _ {A / M / P} .tif 인 이미지에 배포된다. MD5 = e2949f19a0d1102827fce35117c5f08a

- **grid_sizes.csv-** 모든 이미지의 격자 크기

  - ImageId-이미지의 ID

  - Xmax-이미지의 최대 X 좌표
  - Ymin-이미지의 최소 Y 좌표

- **sample_submission.csv-** 올바른 형식의 샘플 제출 파일

  - ImageId-이미지의 ID

  - ClassType-객체 유형 (1-10)
  - MultipolygonWKT-레이블이 지정된 영역으로 WKT 형식으로 표현 된 다중 다각형 지오메트리

- **train_geojson.zip-** 모든 교육 레이블의 geojson 형식 (본질적으로 train_wkt.csv와 동일한 정보 임) 

