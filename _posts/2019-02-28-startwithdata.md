---
layout: post
title: 데이터 속에서 방황하기
tags:
  - 회고
excerpt: "
<i>-데이터에서 출발해서 스토리를 이끌어 내면 돼 -그게 뭔데 -스토리를 이끌어내라고 -그게 어떻게 하는건데</i><br>
데이터 저널리즘팀에서 분석가는 데이터에서 출발해서 스토리를 더듬어가는 사람이었다. 어찌보면 방황이라고 할만한 그 과정과 재미를, 그리고 나름의 강점을 옹호해보고자 한다."
--- 

## 1
‘데이터에서 출발해 스토리를 이끌어 내라 Start with the data, finish with a story” 유럽 저널리즘 센터에서 발간한 데이터 저널리즘 핸드북 첫 번째 권호의 소제목 중 하나이다. 데이터 저널리즘뿐 아니라 데이터 분석 전반에 관심 있는 사람이라면 어떤 형태로든 한 번쯤 들어본 익숙한 구호일 것이다. 이 거창한 구호의 비호 속에서도 데이터에서 출발해 스토리를 더듬어가는 과정은 어쩔 수 없이 ‘내가 잘하고 있는 게 맞는 건가?’ 하는 고통스러운 자아 성찰을 이만 번쯤 수반한다.  

특히 데이터 저널리즘 팀에서 데이터 분석으로 일하는 초기에는 더욱 그랬다. 연구 질문과 데이터 수집 간의 순서가 전통적인 방법론과는 반대인 듯한 인상을 지울 수 없었다. 강연에서 본 전문가들은 뚜렷한 문제의식 없이 우선 데이터를 모으고 보는 접근법을 비판했다. 리포트를 쓸 때도 교수들은 데이터를 보지 않은 상태에서 연구 주제를 잡고 가설을 세운 뒤에 이를 구체적인 연구 질문으로 세분화하기를 요구했다. 데이터에 해당하는 인터뷰나 설문조사를 기획하는 것은 그다음이었다. 연구 질문에 효과적으로 답을 제시해 줄 만한 연구 대상을 선정하고 표집 방법을 선택하여 조사 대상을 좁혀나갔다. 질문이 없는 상태에서는 어떤 집단을 대상으로 조사를 시행할지, 어떤 변수를 측정할지 모르는 까막눈이었다.  

'여전히 낮은 목소리' 시리즈를 기점으로 나는 까막눈에 적응하기 시작했다. 어두움에 익숙해지고 그 속에서 자유롭게 스토리의 단서를 탐색하는 일을 즐기기 시작했다. 이 글에서 그 방황의 과정과 재미를, 그리고 나름의 장점을 옹호해보고자 한다.


## 2
2018년 8월 SBS 데이터 저널리즘 팀 마부작침에서 발행한 '여전히 낮은 목소리'는 8월 14일 위안부 기림의 날을 맞아 위안부 이슈를 영화로 조명해보자는 취지로 기획되었다. 같은 2차 세계 대전의 인권 참사인 홀로코스트에 비해 덜 다뤄진다는 점에서 주목하여, 위안부 이슈와 이를 다룬 영화에 관해서도 관심을 촉구하자는 방향성이었다. 이렇게 ‘홀로코스트 영화와 위안부 영화의 대조’라는 컨셉만 확정한 채, 곧바로 데이터 수집에 돌입했다. 맨땅에 해당하듯이 지금까지 개봉된 모든 홀로코스트 영화와 위안부 영화의 목록을 구축하기 시작했다. 하나도 빠짐없이 일종의 ‘모집단’을 확보하는 것이 중요했다. 데이터의 분포에 대해 짚이는 바가 없기 때문에, 혹은 미리 단정 짓지 않기 때문에 표본을 선정하지 않는다. 파이썬 크롤링에 기대어 최대한 포괄적으로 데이터에 다가갔다.  

홀로코스트를 다룬 영화의 목록은 세계적인 영화정보 사이트 IMDb를 참고했고, 최종적으로 685개를 추렸다. 영화의 제목과 함께 개봉연도, 장르, 상영 시간, 시청등급, 제작국가, 감독, 영화제 수상 및 후보 지명 횟수, 예산, 첫 주 흥행 성적 등의 변수를 수집했다. ‘위안부’를 다룬 영화의 목록은 KMDb(한국영상자료원 한국영화데이터베이스)를 참고했다. 제작된 후 개봉하지 않은 영화를 포함해도 40편이 채 되지 않았다. KMDb에서 공개하는 변수는 모두 정리했다. 장르, 제작연도, 상영 시간, 시청등급, 감독처럼 홀로코스트 영화에 대해 수집된 내역과 대응되는 변수도 있었지만, 배우, 스크린 수, 누적 관객 수처럼 IMDb에서는 제공하지 않는 변수도 있었다. 이 또한 가리지 않고 변수를 최대로 확보했다. 다음 단계에서 제약 없이 여러 변수의 조합을 시도해보는 것만을 고대하며, 이를 악물고 모든 항목을 수집하는 파이썬 코드를 작성해나갔다.

<p align="center"  style="font-size:80%;">
  <img src="{{site.baseurl}}/images/imdb_crawler.png" alt="imdb cralwer code" width="80%"/>
  당시 작성했던 파이썬 크롤링 코드의 일부. 
  <a href="https://github.com/yoongaemii/IMDBcrawler">전체 소스코드 보기</a>
</p>


## 3
그렇다면 어떻게 조합을 실험하고, 스토리를 구상할까? 흔히들 EDA라고 부르는 탐색적 데이터 분석에는 다양한 지침과 팁들이 오가지만, 결국 ‘분석가 본인의 문제의식이 중요합니다’하는 원론적인 말로 귀결되기 마련이다. 내가 이 분석에 임하면서 깨달은 그나마 실질적인 지침은, 그래프를 그리는 걸 두려워하지 말자는 것이었다. 사실 두렵다기보다는 귀찮은 경우가 많다. 수치가 빼곡히 적힌 테이블을 보고 ‘아 대충 어떤 추세인지 알겠다. 단순한데 그래프까지 그릴 필요는 없겠네’ 하고 넘어가는 식이다. 그렇게 우리는 중요한 단서를 무심코 지나가 버린다. 아무리 단순한 기초 통계량도 시각적으로 봤을 때 특이점을 파악하기 쉽다. 그런 점에서 시각화는 데이터와 직관을 잇는 가교의 역할을 한다고 나는 믿는다.  

그렇게 믿는다고 갑자기 그래프 그리기가 안귀찮아진 것은 아니었다. 그래서 그래프 하나에 소요되는 비용을 최소화하는 방법을 꾸준히 마련해 가야겠다는 생각이 들었다. 다음 학기에 데이터 분석 학회를 하면서 시각화 스터디를 기획한 것도, 더 아름답고 효과적인 시각화를 위해서라기보단 간편하고 신속한 시각화를 위해서였다. 단, 여기서 탐색적 시각화는 다른 팀원들에게 보여주거나 발행하는 목적의 시각화와는 확실히 구분해야 한다. 탐색을 위한 그래프는 엉성하더라도 다양하게 그려보는 방향이 좋다. 파이썬이나 R의 유려한 패키지가 아니어도 좋다. 엑셀이어도 좋다. 위안부 영화의 경우 데이터 포인트가 몇 개 되지 않아 손으로 그렸다. 남들에게 보여줄 것이라면 절대 선택하지 않을 요란하고 복잡한 시각적 cue를 사용했다. 무리하게 여러 개의 변수를 하나의 그래프 안에 표현했다. 구분선을 이리 저리 그려보며 퍼즐을 여기 저기 갖다대 보았다.


## 4
그 그래프를 통해 데이터의 특이점을 발견하는 것이 스토리 구성의 첫번째 단계다. 가장 쉬운 예시는 증감의 추세이다. 홀로코스트를 다룬 영화의 수를 연도별로 줄 세워보니, 완만하게 증가세를 그리거나 감소세를 보이는 부분과 달리 1년 새에 급격하게 영화의 수가 증가하는 지점이 있었다. 1942년이었다. 그 이유가 자연스럽게 궁금해진다. 이렇게 추세의 이유와 배경을 리서치하는 과정이 두번째 단계에 해당한다. 홀로코스트 관련 영화가 급증하기 1년 전, 1941년은 미국이 세계 제2차 대전에 참전한 해였다. 제작 국가 별 그래프를 그려보니 1942년 개봉한 32편의 영화 중 30편이 미국에서 제작되었다(이럴 때 모든 항목을 크롤링하길 잘했다는 생각에 뿌듯해진다). 홀로코스트를 포함하여 나치 정부의 반인륜적 행태를 고발하는 영화들은 미국의 전쟁 참여에 명분을 주었다. 즉, 이 시기 제작된 영화들은 미국이 단순히 승리를 위해 전쟁에 임하는 것이 아니라, 평등과 민주주의를 위협하는 적을 상대한다는 의미를 부여하는 기능을 했다. 

<p align="center"  style="font-size:80%;">
  <img src="{{site.baseurl}}/images/movie_byyear.png" alt="위안부 영화와 홀로코스트 영화" width="80%"/>
  홀로코스트 영화와 위안부 영화의 증감 추이
</p>


마법처럼 리서치로 이야기가 완성되는 경우도 있지만, 추가적인 데이터가 필요한 경우도 있다. 앞서 이야기한 1942년 이후로 한 번 더 홀로코스트 영화의 수가 급증했던 해가 1993년이었다.  리서치 결과, 이 때 쯤 홀로코스트가 냉전 이후 공산주의를 대체하며 ‘악’의 묘사로 자리잡았고, 홀로코스트가 그 유럽적인 맥락에서 탈피해 보다 광범위하게 인간의 잔인함을 성찰하는 기능을 수행하게 되었다는 분석이 우세했다. 그리고 그 중심에 쉰들러 리스트의 개봉과 미국의 홀로코스트 박물관 개장이라는 사건이 있었다.

첫번째 이유는 비교적 쉽게 이해할 수 있다. 쉰들러리스트의 개봉과 흥행, 많은 수상으로 말미암아 홀로코스트 소재를 소재로 하는 영화가 더 활발히 제작됐다는 것이다. 그런데 홀로코스트 박물관 개장이 그 유명한 쉰들러리스트 만큼이나 일반 대중들의 마음을 움직였을까? 박물관 하나의 영향력이 영화 제작을 촉진할 만큼 광범위한 영향력이 있다는 사실은 크게 와닿지 않았다. 이런 순간에 이 작업은 나 혼자 배우고 깨달음을 얻기 위한 게 아니라는 점을 되새긴다. 팀원들을 설득하고, 궁극적으로는 기사의 독자들을 설득해야 했다. 그래서 박물관의 영향력을 ‘사람들의 마음에 와닿게 할 수 있는' 자료를 더 찾아 헤맸다. 그 때 마주친 것이 구글 트렌드 데이터다. 구글은 시기 별 검색어의 인기와 연관 검색어 및 키워드를 살펴볼 수 있는 '구글 트렌드’ 기능을 제공한다. 에 지역을 미국으로 한정하고 홀로코스트를 키워드로 검색한 결과,  상위 연관 토픽 및 검색어로 “Holocaust Museum”, “Washington DC”(박물관의 위치) 등이 등장했다.  미국인들에게 홀로코스트 뮤지엄이 홀로코스트에 대한 현대의 기억과 이미지를 형성하는 데 상당한 영향을 줬다는 사실이, 그제야 와닿았다. 


<p align="center"  style="font-size:80%;">
  <img src="{{site.baseurl}}/images/interactive_main.jpg" alt="인터랙티브 시각화" width="80%"/>
  자세한 내용과 일본군 ‘위안부’의 영화화가 궁금하다면? 
  <a href="http://mabu.newscloud.sbs.co.kr/201808murmuring/">인터랙티브 기사 보기</a>
</p>


## 5
그렇게 우리는 홀로코스트의 영화화에 작용한 역사를 훑어볼 수 있었다. 물론 이렇게 구성한 스토리가 우리의 기획의도를 완벽히 따라주길 기대하긴 힘들다. 일본군 ‘위안부’ 영화화의 부진을 홀로코스트 영화와 대조하여 드러내려했던 기획 의도와 달리, 홀로코스트가 대량으로 영화화가 된 것은 생각보다 정치적인 배경이 많이 작용했다는 사실을 발견했다. 이는 데이터가 증명해주고 있었다. 데이터에서 시작하여 문제의식과 스토리를 찾아가는 방법의 한계라면 한계다. 데이터가 우리를 어디로 이끌지 예측할 수 없다는 점.

동시에 강점도 발견했다. 첫번째, 데이터 수집의 과정에서부터 투명성이 보장된다. 홀로코스트 영화와 위안부 영화에 대해서 어떠한 전제도 없이 ‘전수 조사’를 하듯 데이터를 수집했다. 분석 과정에서 편향에 비해 간과되는 수집에서의 편향을 방지할 수 있다. 어떤 분석가는 자신만의 문제 의식을 가지고 촘촘하게 데이터 수집 과정을 설계하고자 한다. 이를 자의적이라며 비판하고 싶은 것이 아니다. 이를 전통적인 방법론으로 본다면 보다 투명한 과정을 원하는 지금의 독자들에게는 새로운 방법론이 필요할 수도 있다는 이야기다. 두번째로, 배경 지식이 비교적 부족한 상태에서도 분석에 돌입할 수 있다는 점이다. 배경지식의 부족을 앞서 언급한 ‘현실적인 제약’ 중 일부로 볼 수 있겠으나, 많은 분석가가 한 달 마다 새로운 도메인에 대한 분석을 요구받는 현 상황을 쉬쉬하며 외면해서는 안된다. 이는 예외적인 상황이거나 안타까운 현실적 제약이 아니다. 어떤 분석가에게 이는 일의 숙명이며, 새로운 도메인을 알아가는 과정도 데이터 분석의 본질이다. 그런 상황에서 나는 데이터에서부터 도메인을 배우고 리서치를 시작할 단서와 출발점을 얻는 보다 실질적인 지침이 필요하다고 생각하며, 이번에 시도한 이 방법도 이를 탐구하고 있는 사람들에게 도움이 되었으면 좋겠다.
