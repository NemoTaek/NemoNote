---
title: "복잡도(Complexity)"
tags:
  - 시간복잡도
  - 공간복잡도
---

자료구조 글을 작성할 때, 시간복잡도와 공간복잡도 이야기가 나와서 이에 대한 글을 작성해보도록 하겠다.  

복잡도란 얼고리즘 성능 분석을 판단하는 요소중 하나이며, 이를 가장 큰 우선순위로 두고 있다.  
복잡도에는 시간복잡도와 공간복잡도로 나누어지며 표기법은 Big-O 표기법을 사용한다.  
<br>

1. 시간복잡도  

    시간복잡도란 함수를 실행했을 때 결과를 반환할 때까지 걸리는 실행시간이 아닌, 알고리즘에 사용되는 **연산 횟수의 총량**이다.  
    그렇다면 왜 실행시간이 아닌것일까?  
    실행시간으료 판단한다면 한 컴퓨터에서 실행할 때마다 결과값이 계속 달라지고, 큰 편차가 있을 수 있기 때문이다. 이렇게 되면 객관성이 떨어져 신뢰성을 잃을 수 있다.  
    그러므로 객관성을 얻기 위해서 모든 컴퓨터에서 동일한 결과를 얻을 수 있는 연산횟수 또는 시행횟수로 판단하게 된 것이다.  

    그렇다면 시간복잡도를 구하는 방법은 무엇일까?  
    중,고등학교 때 배운 방정식을 떠올려보자.  
    변수 x에 대하여 x의 차수에 따라 그래프의 기울기가 변하는 것을 배웠다. 컴퓨터에서도 그대로 적용된다.  
    input에 사용되는 변수 x를 얼마나 함수를 연산하나에 따라 시간 복잡도가 결정된다.  

    예시를 한번 보자.  

    ```
    let a = 1;
    let b = 2;
    return a + b;
    ```
    a와 b의 값이 주어졌을때, 이를 더한 값을 반환하는 함수가 있다고 하자. 여기서 실행 횟수는 몇번이 될까?  
    그렇다. a값을 읽고, b값을 읽고, a+b연산을 1번 하므로 1 + 1 + 1번만 하게 된다.  
    여러번 반복하지 않고 정해진 몇번만 하기 때문에 이를 상수번 실행한다고 하고, 이를 Big-O 표기법으로 표현하면 O(1)이라고 한다.  

    다음 예시이다.  

    ```
    let a = [1, 2, 3, 4, 5];
    for(i=0 ; i<a.length ; i++){
      if(a[i] === 3){
        return 'good'
      }
    }
    ```
    이렇게 3을 찾으면 good을 반환하는 함수가 있다고 하자. 여기서 실행 횟수는 몇번이 될까?  
    a라는 배열 내에서 3이라는 숫자를 찾기 위해 a의 길이만큼 반복해서 탐색을 하고 있다. 그러므로 상수번 실행되는 것이 아닌 n번 실행되는 것이라 볼 수 있다. 이를 표현하면 O(n)으로 표현할 수 있다.  

    예시를 한번 더 보자.  

    ```
    for(i=2 ; i<9 ; i++){
      for(j=1 ; j<9 ; j++){
        console.log(i x j = i*j)
      }
    }
    ```
    2단에서 9단까지 표시하는 구구단 함수가 있다. 여기서 실행 횟수는 몇번이 될까?  
    먼저 2단을 표시하기 위해 i=2에서 j=1 ~ 9까지 한번 반복할 것이다.  
    그리고 3단을 표시하기 위해 i=3에서 j=1 ~ 9까지 반복할 것이고, n단을 표시하기 위해 j=1 ~ 9까지 반복할 것이다.  
    그렇다면 n번 실행되는 함수 내에서 n번을 다시 실행하므로 n * n번 실행하므로 n^2번 실행되는 것이라 볼 수 있다. 이를 표현하면 O(n^2)으로 표현할 수 있다.  
<br>
    이제 생각해보자. 앞선 예시에서 O(1), O(n), O(n^2)가 있었는데, 어떤 알고리즘의 연산 횟수가 제일 적을까? 그렇다. O(1)이 제일 적고, O(n^2)가 제일 많을 것이다.  
    이를 통해서 연산 횟수가 적을수록 알고리즘의 성능이 좋다는 것을 알 수 있다.  
    다음은 시간복잡도가 높은 순으로 배열한 것이다. 참고하도록 하자.  
    O(1) < O(log(n)) < O(n) < O(nlog(n)) < O(n^2) < O(2^n) < O(n!) < O(n^n)  
    참고로 O(n^2)이상이 되는 순간 알고리즘의 성능이 급격하게 떨어지므로 최대한 그 안쪽으로 구현하는 것을 목표로 하자.  
<br>
    그런데 말입니다.(...?)  
    어떤 알고리즘이 있다고 했을때, 실행할 때마다 모두 같은 연산 횟수가 나올까요?  
    조건에 따라서 횟수가 적을 수도 있고, 모든 횟수를 다 채울 수도 있지 않을까요?  
<br>
    구구단을 봅시다.  
    만약 조건이 2 x 1을 찾는 문제였다고 하자. 그러면 시간복잡도는 O(1)이 될 것이다.  
    아니면 조건이 4 x 5를 찾는 문제였다고 하자. 그러면 시간복잡도는 O(n)이 될 것이다.  
    아니면 조건이 9 x 9를 찾는 문제였다고 하자. 그러면 시간복잡도는 O(n^2)이 될 것이다.  
<br>
    이처럼 한 알고리즘에서도 여러가지 연산 횟수가 나올 수 있다.  
    처음 조건처럼 시작하자마자 딱 맞아 떨어지는 경우를 **최선의 경우(Best Case)**라 하고,  
    **일반적인 경우를 Average Case**라 하고, 제일 마지막에 나오는 경우를 **최악의 경우(Worst Case)**라 한다.  
    보통 알고리즘에서 시작하자마자 딱 맞아 떨어지는 경우는 거의 없으므로 보통 Average나 Worst를 기준으로 시간 복잡도를 결정한다.  
<br>
2. 공간복잡도  

    공간복잡도란 알고리즘을 실행시켜서 완료하는데 필요로 하는 **자원의 공간의 양**이다.  

    예시를 보자.  

    ```
    function factorial(n)
    {
        if(n > 1) return n * factorial(n - 1);
        else return 1;
    }
    ```
    팩토리얼 예제이다. n이 1이하일 때까지 함수가 재귀적으로 호출되므로 스택에 n부터 1까지 모두 쌓이게 된다. 그러므로 공간복잡도는 O(n)이 된다.  

    ```
    function factorial(n)
    {
        int i = 0;
        int fac = 1;
        for(i = 1; i <= n; i++)
        {
            fac = fac * i;
        }
        return fac;
    }
    ```
    같은 팩토리얼 예제이다. 여기서는 n의 값과 상관없이 i, fac, n의 변수에만 값이 저장되므로, 공간복잡도는 O(1)이 된다.  

    이를 바탕으로 좋은 알고리즘이란 시간복잡도와 공간복잡도가 최소가 되는 알고리즘이라고 말할 수 있을 것 같다.  
    하지만, 시간과 공간은 반비례하는 경향이 있어 시간 복잡도를 더 우선으로 판단한다.(역시 프로그램은 용량보다는 빨라야 하나보다...)  

<br>
다음 링크는 각각의 자료구조에 대한 시간복잡도 표이다. 참고하면 좋을 것 같다.  
[시간복잡도](https://www.bigocheatsheet.com/)