# QA-model
2022년 2학기 데이터사이언스 특론 term-project
kobert & stable diffusion을 이용하여 대회형 질문에 대한 정답과 이미지를 같이 보여주는 Q&A model 개발

# 환경
google colab

# Member
* 손창진 (keyword extraction, crawling, Q&A model, Translation model)
* 김준석 (keyword extraction, crawling, Image generation, flask web 개발)

# Data
#### kobert
* ai-hub의 뉴스기사기계독해 데이터셋
* https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=577
#### kogpt
* ai-hub의 일상생활 및 구어체 한-영 번역 병렬 말뭉치 데이터
* https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=71265

# Model
![image](https://user-images.githubusercontent.com/93864811/213376177-e8d7d84d-3935-445a-a222-bab8f3868f8d.png)
* User에게 question을 하나 질문으로 입력받고 정답과 정답에 해당하는 이미지를 stable diffusion으로 생성 후 정답과 같이 화면에 띄어줌

# Keyword 추출 & Crawling
* User에게 입력받은 질문으로부터 주요 keyword를 추출한 후 추출된 키워드를 기반으로 빅카인즈(뉴스기사 검색사이트)에서 뉴스기사 검색 후 크롤링
* keyword 추출시 빅카인즈 내에 키워드 추출 프로그램 사용 후 중복제거해서 사용
* 정답의 정확도를 높이기 위해 10개의 뉴스기사를 수집
* https://www.bigkinds.or.kr/

# Question & Answering model
* ai-hub의 뉴스기사기계독해 데이터셋을 사용하여 kobert & multilingual-bert를 fine-tunning 후 비교

![image](https://user-images.githubusercontent.com/93864811/213378798-72bdffa1-0c5d-4823-bf33-ae2015dd6f5d.png)
* multilingual-bert의 성능이 좀 더 좋았지만 tokenizer관련 issue가 있어 kobert model 9번째 epoch의 파라미터 사용
* 실제 사용시에는 정답의 정확도를 높이기 위해 앞에서 수집한 10개의 기사에 대하여 각각 정답과 확률을 구하고 가장 확률이 높은 하나의 정답을 반환

# Translation
* stable diffusin 2 모델이 영어만 지원하기 때문에 한국어 -> 영어 번역이 필요
* skt에서 개발한 kogpt2 모델을 fine-tunning하여 사용해 보았을때 문장수준에서 번역은 어느정도 잘 하였으나 고유명사나 사람이름 등을 인식하지 못함
* 고유명사를 인식하는 모델까지 개발해서 사용하고 싶었지만 시간적인 문제와 한-영 고유명사 매핑 데이터를 수집하는데 어려움이 있어 다른 번역모델 사용

#### Kogpt fine-tunning Result
* 문장수준에서의 번역

![image](https://user-images.githubusercontent.com/93864811/213381190-ca508c5b-779a-4a17-b046-34a8756bf07a.png)

* 고유명사 & 사람이름 인식에서의 문제

![image](https://user-images.githubusercontent.com/93864811/213381336-22f2c9c0-5d6a-4ab8-affa-dae3d91d9a00.png)

#### QuoQaGo 번역모델
* 앞선 문제들로 인하여 어느정도 잘 학습된 KE-t5기반 번역모델
* fine-tunning 없이 사용
* https://huggingface.co/spaces/QuoQA-NLP/QuoQaGo
![image](https://user-images.githubusercontent.com/93864811/213381880-d814c745-7fc5-4edc-aec2-549bf3ab0c93.png)


# Image generation
* stable diffusion2 model 사용 이유
* 데이터셋을 구하기 힘들어 추가적인 튜닝없이 변수들만 조정하여 사용
![image](https://user-images.githubusercontent.com/93864811/213382409-c1b175d2-1ab1-4fc2-88b1-513dee66da67.png)

# Results
* flask를 이용하여 웹페이로 구현

* 시작페이지

![image](https://user-images.githubusercontent.com/93864811/213382834-e300cc73-71dc-467c-8328-19dd6ce46a72.png)

* 결과 1

![image](https://user-images.githubusercontent.com/93864811/213382903-75536ab7-786c-4313-a809-26f36538fd54.png)

* 결과 2

![image](https://user-images.githubusercontent.com/93864811/213382943-0048cc20-3a47-4fea-9767-bf7d1f2b9195.png)





