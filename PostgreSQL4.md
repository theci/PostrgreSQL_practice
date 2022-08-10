# Chapter 4 고급 SQL 명령
- 각종 시간 정보를 보는 법
  SHOW ALL - 실행 시간 값을 보여주는 매개변수
  SHOW TIMEZONE - 작업하는 곳에 해당하는 표준 시간대
  SHOW NOW() - 날짜와 시간 및 표준시간대 정보를 알 수 있음
  SHOW TIMEOFDAY()
  SELECT CURRENT_TIME
  SELECT CURRENT_DATE

- EXTRACT() - 년, 월, 일, 주, 분기별로 추출할 수 있다.
  SELECT EXTRACT(YEAR FROM payment_date) AS year
  FROM payment

- AGE() - 타임스탬프 내에서 현재까지의 시기를 계산해서 알려준다.
  SELECT AGE(payment_date)
  FROM payment


- TO_CHAR() - 일자 유형을 글자로 바꿔주는 함수
  SELECT TO_CHAR(payment_date, 'MM-dd-YYYY')
  FROM payment

- 어떤 달에 지급이 이루어졌을까요?
  SELECT DISTINCT(TO_CHAR(payment_date,'MONTH'))
  FROM payment

- 월요일에는 지급이 얼마나 발생했을까요? (Postgre에서 한 주의 시작 일요일은 인덱스가 0이다.)
  SELECT COUNT(*)
  FROM payment
  WHERE EXTRACT(dow FROM payment_date) = 1

- 수리연산 - 대여료는 대여 원가의 몇 퍼센트일까?
  SELECT ROUND(rental_rate/replacement_cost,2)*100 AS percent_cost
  FROM film

- 대체 비용 보증금 10퍼센트를 낮추기로 했을 때 가격
  SELECT 0.1 * replacement_cost AS deposit
  FROM film

- 문자열 길이 알아보기
  SELECT LENGTH(first_name) FROM customer

- 문자열 합치기
  SELECT first_name || ' ' || last_name AS full_name
  FROM customer

- 문자열 대문자 만들기
  SELECT upper(first_name) FROM customer

- 이름 가지고 이메일 만들어보기
  SELECT LOWER(LEFT(first_name,1)) || LOWER(last_name) AS custom_email
  FROM customer

- 서브쿼리
  평균보다 높은 학생 구하기
  SELECT student,grade FROM test_scores
  WHERE grade > (SELECT AVG(grade) FROM test_scores)

  대여료가 평균보다 높은 영화 도출하기
  SELECT title,rental_rate
  FROM film
  WHERE rental)rate >
  SELECT AVG(rental_rate) FROM film

- 다른 표에 있는 학생의 시험 성적표에서 학생과 성적을 도출
  SELECT student,grade FROM test_scores
  WHERE student IN
  (SELECT student FROM honor_roll_table)

  어떤 기간에 반납된 영화 이름을 확인하기
  SELECT film_id,title
  FROM film
  WHERE film_id IN
  (SELECT inventory.film_id
  FROM rental
  INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
  WHERE rental.return_date BETWEEN '2005-05-29' AND '2005-05-30')
  ORDER BY film_id

- 어떤 행이 서브 쿼리로 도출되었는지 확인
  SELECT column_name
  FROM table_name
  WHERE EXISTS
  (SELECT column_name FROM table_name WHERE condition);

  11달러 초과로 한 번 이상 지급한 고객의 이름과 성을 보기
  SELECT first_name,last_name
  FROM customer AS c
  WHERE EXISTS
  (SELECT * FROM payment as p
  WHERE p.customer_id = c.customer_id
  AND amount > 11)

- 셀프 조인 - 같은 표 내 여러 열의 여러 값을 비교할 때 유용함
  SELECT tableA.col,tableB.col
  FROM table AS tableA
  JOIN table AS tableB ON
  tableA.some_col = tableB.other_col

  직원 아이디와 리포트 아이디로 발송자name와 수신자 이름rep 표로 만들기
  SELECT emp.name,report.name AS rep
  FROM employees AS emp
  JOIN employees AS report ON
  emp.emp_id = report.emp_id

  같은 영화 시간을 가진 영화 짝을 지어보기
  SELECT f1.title, f2.title, f1.length
  FROM film AS f1
  INNER JOIN film AS f2 
  ON f1.film_id != f2.film_id
  AND f1.length = f2.length
