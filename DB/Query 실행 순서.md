#Query 처리, 실행 순서

## SQL 문법 순서
```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
```

## SQL 문법 처리, 실행 순서
```sql
1. FROM      -- 조회할 테이블에 접근 한다.
2. ON        -- 조인 조건을 확인한다.
3. JOIN      -- 조인 조건을 통해 조인한다. 
4. WHERE     -- 전체 데이터에 조건문을 건다.
5. GROUP BY  -- 결과를 그룹핑 하고
6. HAVING    -- 그룹화 조건을 건다.
7. SELECT    -- 원하는 컬럼을 조회한다.
8. DISTINCT  -- 중복값을 제거한다.
9. ORDER BY  -- 결과를 정렬한다.
```

## ROWNUM
ROWNUM을 가져오기 위해서 ORDER BY 해서 SELECT 하면 ROWNUM이 섞여있는것을 볼 수 있다.   
그리고 WHERE절로 특정 ROWNUM을 가져오지 못하는 경우도 있다.   
그 이유는 ROWNUM의 할당 위치가 WHERE 절 다음이기 때문이다.
