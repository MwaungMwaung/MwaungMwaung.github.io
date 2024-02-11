프로그래머스 '서울에 위치한 식당 목록 출력하기' 문제를 풀면서 아래와 같은 코드를 작성했다.

```sql
SELECT inf.REST_ID, REST_NAME, FOOD_TYPE, FAVORITES, ADDRESS, ROUND(AVG(REVIEW_SCORE), 2) AS SCORE
FROM REST_INFO AS inf LEFT JOIN REST_REVIEW AS rv ON inf.REST_ID = rv.REST_ID
WHERE inf.ADDRESS LIKE '서울%'
GROUP BY inf.REST_ID
HAVING SCORE IS NOT NULL
ORDER BY SCORE DESC, FAVORITES DESC;
```
아무 문제 없이 정답이었는데...
잘 생각해보니 이상한 점이 있다.

SQL 실행 순서에 따르면 'SELECT'보다 'HAVING'이 먼저 수행된다.
그런데, 내가 짠 코드를 보니 SELECT에서 붙힌 별칭이 HAVING에서 사용되고 있었다.
이게 어떻게 된 일일까? 싶어서 찾아봤다.

결론은 이거다.
'MySQL'은 'SELECT'절에서 정의된 별칭을 'GROUP BY'와 'HAVING'에서 참조할 수 있도록 구현되어 있어서다.
oracle, mssql 등 **다른 문법에선 보장할 수 없는 사용법**이란 걸 꼭 알아둬야겠다.