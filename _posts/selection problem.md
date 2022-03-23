---
title: 컴퓨터알고리즘 3주차 정리
layout: post
post-image: https://ww.namu.la/s/146b60d7b4733ab70547914b288ab6449e63a164589c674cab5ed0d14c96bf5be9ab98261cbf419b71a0f10c4897a1082b7b94a213af1b1659fe4f6787ddc8844f406062377cb13aab18d0f145a2d3e1
description: 컴퓨터알고리즘 3주차 정리
tags:
- selection problem
- 선택 문제
- 퀵정렬
use_math: true
---

# Selection problem 구현

## 첫번째 시도
- 수업시간에 들었듯이 퀵 정렬과 혼합하여 만들었다.
- 처음에 배열을 만들고, **k**라는 배열 중 k번째 작은 숫자를 지정하였다.
- 그 후 배열의 왼쪽 끝과 오른쪽 끝인 **left**,**right**를 지정하여 **selection problem**이라는 함수에 넣었다.
- 정렬은 랜덤으로 구한 pivot을 기준으로 하는 퀵 정렬의 방법을 따랐다.

### 파이썬으로 진행하였습니다.
``` py
from random import randint

def selection_problem(arr,left,right,k):
    # 수업시간에 배웠던 퀵 정렬 이론대로 구현해 보았다
    # 블로그를 최대한 참고하지 않고 만들어 보려고 해서 코드가 조금 더럽다. 
    pivot = randint(left,right)
    if pivot != left:
        arr[pivot],arr[left]=arr[left],arr[pivot]
    p=left+1
    q=right
    
    while(p<=q):
        if p>q:
            break
        if arr[p]<arr[left]:
            if arr[q]>arr[left]:
                arr[p],arr[q] = arr[q],arr[p]
            else: q-=1
        elif arr[q]>arr[left]:
            if arr[p]<arr[left]:
                arr[p],arr[q] = arr[q],arr[p]
            else: p+=1
        else: 
            p+=1
            q-=1
   # 이후에는 피벗보다 작은 그룹의 크기와 k를 비교하여 k번쨰 숫자를 찾아나갔다.
    arr[left],arr[q]=arr[q],arr[left]
    small_group=q-left
    if (k==small_group+1):
        return arr[q]
    elif (k<=small_group):
        return  selection_problem(arr,left,q-1,k)
    else:
        return  selection_problem(arr,q+1,right,k-(small_group+1))

arr=[6,3,11,9,12,2,8,16,18,10,7,14]
k=7
print(selection_problem(arr,0,len(arr)-1,k))

```
## 첫번쨰 시도 결과
- k번째 작은 수를 찾아야 되는데
- **k번째 큰 수가 나온다..**
- selection problem의 k넣는곳에 len(arr)-k+1 해서 넣으면 답이 나오긴 하지만,
- 이럴려고 코드 짠게 아니므로 다시 해보기로 하였다.

## 두번째 시도
- 2,3,4로 간단하게 예시로 세우고, 노트에 써서 내 코드대로 움직여봤다.
- 그러자 놀랍게도! pivot 보다 큰 수가 작은 그룹에 들어가는 것이였다.
- 그렇다 나는 부등호를 반대로 쓴 것이였다 .
- 별로 놀랍진 않지만 놀랍다고 쓴건 당연한건데 모르고 있던 나 자신에 대한 놀람이다.
- 해결법을 알았으니 바로 코드를 고쳤다.

``` py
from random import randint

def selection_problem(arr,left,right,k): 
    pivot = randint(left,right)
    if pivot != left:
        arr[pivot],arr[left]=arr[left],arr[pivot]
    p=left+1
    q=right

    # 이 부분에서 부등호가  반대로 되어있었다.
    
    while(p<=q):
        if p>q:
            break
    #if arr[p]<arr[left]에서 수정        
        if arr[p]>arr[left]:
    #if arr[q]>arr[left]:에서 수정
            if arr[q]<arr[left]:
                arr[p],arr[q] = arr[q],arr[p]
            else: q-=1
    #if arr[q]>arr[left]:에서 수정
        elif arr[q]<arr[left]:
    #if arr[p]<arr[left]에서 수정   
            if arr[p]>arr[left]:
                arr[p],arr[q] = arr[q],arr[p]
            else: p+=1
        else: 
            p+=1
            q-=1
    arr[left],arr[q]=arr[q],arr[left]
    small_group=q-left
    if (k==small_group+1):
        return arr[q]
    elif (k<=small_group):
        return  selection_problem(arr,left,q-1,k)
    else:
        return  selection_problem(arr,q+1,right,k-(small_group+1))

arr=[6,3,11,9,12,2,8,16,18,10,7,14]
k=7
print(selection_problem(arr,0,len(arr)-1,k))

```

## 두번쨰 시도 결과
- 원하는 값이 잘 나온다!! 성공했다.
- 후에 찾아봤지만 파이썬으로 selection problem을 구현한 블로그는 거의 없었다.
- **교수님이 수업 때 설명을 잘해주셔서 야무지게 구현 할 수 있었던 것 같다.**

## 오늘의 교훈
- 머릿속으로 코드의 과정을 생각하면서 코딩하자
- 안그러면 나중에 차차 생각하다 꼬인다.
- 부등호를 잘 보자.
