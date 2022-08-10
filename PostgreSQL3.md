# 섹션3 Joins

- AS문은 제일 마지막에 할당되기 때문에 AS문으로 조건문(Having같은)에 적용할 수 없다.

## INNER JOIN은 테이블들을 결합하여 두 테이블 모드에서 매칭되는 레코드를 출력(교집합)
    SELECT * FROM Table A
    INNER JOIN Table B
    ON TableA.col_match = TalbeB.col_match

- patment 테이블과 customer 테이블을 결합하여 특정 결제와 관련된 고객 이메일을 구하는 방법 
    SELECt payment_id,payment.customer_id,first_name
    FROM payment
    INNER JOIN customer
    ON payment.customer_id = customer.customer_id

## FULL OUTER JOIN은 테이블에서 모든 것을 포함한다.
    SELECT * FROM TableA
    FULL OUTER JOIN TableB
    ON TableA.col_match = TableB.col_match

- 각 테이블에 고유한 값만 출력)-(교집합을 포함하지 않는 합집합)
    SELECT * FROM TableA
    FULL OUTER JOIN TableB
    ON TableA.col_match = TableB.col_match
    WHERE TableA.id IS null OR
    TableB.id IS null

- 결제 테이블과 고객 테이블을 비교해서 결제되지 않은 고객 정보 삭제하기
    SELECT * FROM customer
    FULL OUTER JOIN payment
    ON customer.customer_id = payment.customer_id
    WHERE customer.customer_id IS null
    OR payment.payment_id IS null

# LEFT OUTER JOIN
- 테이블 A 정보만 가져오기
    SELECT * FROM TableA
    LEFT OUTER JOIN TableB
    ON TableA.col_match = TableB.col_match

- 테이블B가 포함되지 않는 고유한 테이블A 정보만 가져오기
    SELECT * FROM TableA
    LEFT OUTER JOIN TableB
    ON TableA.col_match = TableB.col_match
    WHERE TableB.id IS null

- film테이블에서 inventory테이블에 있는 정보 제외하기 가져오기
    SELECT film.film.id,tiltle,inventory_id,store_id
    FORM film
    LEFT JOIN inventory ON
    inventory.film_id = film.film_id
    WHERE inventory.film_id IS null

- RIGHT JOIN은 LEFT JOIN과 순서만 바뀐 형태이다.
  테이블A가 포함되지 않는 고유한 테이블B 정보만 가져오기
    SELECT * FROM TableA
    RIGHT OUTER JOIN TableB
    ON TableA.col_match = TableB.col_match
    WHERE TableB.id IS null

- UNION 연산자는 두 결과를 직접 붙인다.
    SELECT column_name(s) FROM table1
    UNION 
    SELECT column_name(s) FROM table2

- 캘리포니아에 살고있는 고객들의 이메일을 구해라
    SELECT district,email FROM address
    INNER JOIN customer 
    ON address.addrss_id = customer.address_id
    WHERE district = 'California'

- 닉 월버그가 나오는 모든 영화 목록을 구하라
    SELECT title,first_name,last_name FROM actor
    INNER JOIN film_actor
    ON film_actor.actor_id = actor.actor_id
    INNER JOIN film
    ON film_actor.film_id = film.film_id
    WHERE first_name = 'NICK' AND last_name = 'Wahlberg'
