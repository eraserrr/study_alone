## 그리디 알고리즘
미리 정한 기준에 따라서 매번 가장 좋아보이는 답을 선택하는 알고리즘
최적화 문제를 푸는데 사용
동적계획법보다 효율적이지만 반드시 최적의 해를 구하지는 못한다

### 알고리즘
1. 해 선택 : 지금 당시에 가장 최적인 해를 구한뒤, 이를 집합에 추가
2. 적절성 검사 : 새로운 부분해 집합이 적절한 지 검사
3. 해  검사 : 새로우 부분해 집합이 문제의 해가 되는지 검사. 아니라면 1번으로 감

### 선택조건
- 앞의 선택이 이후의 선택에 영향을 주지 않는 조건
- 문제에 대한 최종 해결 방법이 부분 문제에 대해서도 또한 최적 문제 해결 방법이라는 조건

### 프로그래머스 - 구명보트
```
public int solution(int[] people, int limit) {
    int answer = 0;
    int[] check = new int[people.length];

    Arrays.sort(people); //배열 오름차순 정렬
    for(int i=0;i<check.length;i++) // check 배열 1로 초기화
        check[i] = 1;

    for(int i=0;i<check.length-1;i++){ //i 가 0부터 n-1
        if(check[i]==0)
            continue;
        int idx= -1;
        for(int j=i+1;j<check.length;j++){ // j는 1부터 n까지
            if(check[j]==0)
                continue;
            //System.out.println(i+","+j);

            if(people[i] + people[j] <= limit){
                idx = j;
            }
        }
        if(idx==-1) { // 더할만한 인자가없음
            //System.out.println(i + "->" + i);
            answer++;
            continue;
        }
        check[idx] = 0;
        //System.out.println(i+","+idx+"->"+people[i] + people[idx]);
        answer++;
    }
    if(check[check.length-1]==1)
        answer++;
    return answer;

}
```
  
처음 코드였는데 시간초과가 나서 이거보다 짧게 할 방법이 떠오르지않아서 다른 소스코드를 참고해보았다<br>
소스코드를 바꿔서 돌려보았다
```
public int solution(int[] people, int limit) {
    int answer = 0;
    Arrays.sort(people); //배열 오름차순 정렬

    int i=0;
    for(int j=people.length-1;j>=i;j--){
        if(i==j) {
            answer++;
            break;
        }
        if(people[i]+people[j]<=limit){
            i++;
            answer++;
        }
        else
            answer++;
    }
    return answer;
}
```
됐다!!<br>
이렇게하니깐 확실히 시간복잡도가 줄어들 것 같다! 왜이생각을 못했지<br>
이 코드에서 주의할점은 i==j 가 되는 시퀀스에서는 어차피 남은 개수가 하나일때 경우이므로 처리를 따로 해줘야한다는 것이다. <br>

### 프로그래머스 선연결하기

싸이클 검사를 어떻게 해야하는지 생각이안나서 다른 사람의 코드를 참고하였다<br>
먼저 처음에 cycle 이라는 배열을 정점 개수만큼 선언을 해놓고, 각자 자기자신(여기서는인덱스)값으로 초기화한다<br>
이것의 의미는 자기자신이 부모라는 뜻이다 <br>
그런다음에 간선이 하나씩 추가될때마다, 각 정점에서 각자의 부모를 find함수에서 찾아보고, 
각자의 부모가 다르다면 연결이 가능하고, 각자 부모가 같다면 이 간선은 어떤 형태로든 연결되어있는 것이기 때문에<br>
이 케이스에서는 연결을 하지 않는다.<br>
find 함수는 아래와 같다.
```
int find(int node){
    if(node==cycle[node]){
        return node;
    }
    else{
        return cycle[node] = find(cycle[node]);
    }
}
   ``` 

