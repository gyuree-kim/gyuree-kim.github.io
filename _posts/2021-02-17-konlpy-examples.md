---
title: "[nlp][konlpy] KoNLPy okt 간단한 예제 모음"
date: 2021-02-17 15:46
categories:  nlp
---

한국어 자연어처리에 필수인 `koNLPy`의 셋팅을 마치고 간단한 예제를 따라해봅시다. `anaconda`를 통해 필요한 패키지들을 설치한 후 `pycharm`에서 아나콘다 가상환경을 기반으로 돌렸습니다.

### Okt 를 이용한 간단한 예제

* `main.py` 전체 구조
주석 처리된 부분에 코드를 하나씩 추가해보면서 터미널에 찍히는 결과를 확인하면 됩니다! super easy!(파이썬 만세)

    ```python
    from konlpy.tag import Okt

    def konlpy_example():
        # Okt 객체 선언
        okt = Okt() 

        # ...
        # put your codes here...
        # ...

    if __name__ == '__main__':
        konlpy_example()
    ```

*  `morphs` 형태소 단위로 구문을 분석해준다.
```python
print(okt.morphs("안녕하세요 저는 멋진 파이썬입니다. 떡볶이가 먹고 싶어요."))
```

* `nouns` 명사를 추출해준다.
```python
print(okt.nouns("안녕하세요 저는 멋진 파이썬입니다. 떡볶이가 먹고 싶어요."))
```

* `nouncs` 띄어쓰기가 없어도 명사를 잘 추출합니다.
```python
print(okt.nouns("띄어쓰기안할거야명사추출하나보자"))
```

* `phrases` 어절만 추출해준다.
```python
print(okt.phrases("안녕하세요 저는 멋진 파이썬입니다. 떡볶이가 먹고 싶어요."))
```

* `pos(phrase, norm=False, stem=False)` 형태소 단위로 자른 다음에 각 품사들을 tagging 해서 list 형태로 리턴해준다.
```python
print(okt.pos("안녕하세요 저는 멋진 Python입니다. 떡볶이 먹고 싶어요~~ㅋㅋㅋ."))
```