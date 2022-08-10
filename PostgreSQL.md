# 섹션 1 SQL 구문 기초

- customer테이블에서 원하는 열 출력

    SELECT fisrt_name, last_name, email FROM customer;

- film테이블에서 중복된 값이 없는(=고유한) 열 출력

    SELECT DISTINCT rating FROM film;

- payment테이블에서 중복된 값이 없는 행의 개수 출력

    SELECT COUNT(DISTINCT(amount)) FROM payment’;

- customer테이블에서 조건 적용해서 열 출력

    SELECT email FROM customer
    WHERE first_name = ‘Nancy’
    And last_name = ‘Thomas’;

    SELECT phone FROM address
    WHERE address=’259 Ipoh Drive’;

- 가장 먼저 결제한 ID 정렬하고 최상위 10개 출력

    SELECT customer_id FROM pament
    ORDER BY payment_date ASC
    LIMIT 10;

- 상영시간이 가장 짧은 영화 5편의 제목 출력

    SELECT title, length FROM film
    ORDER BY length ASC
    LIMIT 5;

- 영화가 50분 미만일 때 관객이 선택할 수 있는 영화의 수 출력

    SELECT COUNT(title) FROM film
    WHERE length ≤ 50;

- 8달러에서 9달러 사이에 결제된 횟수 출력

    SELECT COUNT(*) FROM payment
    WHERE amount BETWEEN 8 AND 9;

- 2007년 2월 1일에서 15일 00시까지 결제 출력

    SELECT * FROM payment
    WHERE payment_date BETWEEN ‘2007-02-01’ AND ‘2007-02-15’

- 성이 John이거나 Jake거나 Julie인 고객 출력

    SELECT * FROM customer
    WHERE first_name IN (’John’, ‘Jake’, ‘Julie’);

- 가격이 0.99, 1.98. 1.99이 아닌 수량 출력

    SELECT * FROM customer
    WHERE amount NOT IN(0.99,1.98,1.99);

- J로 시작하는 이름을 가진 사람의 수 출력

    SELECT * FROM customer
    WHERE first_name LIKE ‘J%’;

    - ILIKE는 대소문자 구분없이 검색할 수 있다.
    - %는 아무것도 없어도 검색 가능

- 성에 her이 들어가지 않는 고객 출력

    SELECT * FROM customer
    WHERE first_name NOT LIKE ‘_her%’;

- 성이 A로 시작하고 이름이 B로 시작하지 않는 고객 출력

    SELECT * FROM customer
    WHERE first_name LIKE ‘A’ AND last_name NOT LIKE ‘B%’;

- 5달러보다 큰 금액을 결제한 거래는 몇 건인지 출력

    SELECT COUNT(amount) FROM payment
    WHERE amount >5;

- 이름이 P로 시작하는 배우는 몇 명인지 출력

    SELECT COUNT(*) FROM actor
    WHERE first_name LIKE ‘P%’;

- 고객 주소에서 중복되지 않는 고유한 지역은 몇 개인지 출력

    SELECT COUNT(DISTINCT(district))
    FROM address;

- R등급이고 교환 비용이 5달러와 15달러 사이인 영화는 몇 편인지 출력

    SELECT COUNT(*) FROM film
    WHERE rating = ‘R’
    AND replacement_cost BETWEEN 5 AND 15;

- 제목에 Truman이라는 단어가 있는 영화는 몇 편인지 출력

    SELECT COUNT(*) FROM film
    WHERE title LIKE ‘%Truman%’;