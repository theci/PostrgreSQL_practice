# 섹션 2 GROUP BY문, HAVING문

- 최소 교체 비용이 얼마인지 출력

    SELECT MIN(replacement_cost) FROM film;

- 소수점 2째 자리 반올림 하여 평균 교체 비용 얼마인지 출력

    SELECT ROUND(AVG(replacement_cost),2)
    FROM film;

    - 평균 값을 출력하면 소수점이 크게 나오기 때문에 ROUND를 써야한다.

- 총 영화 교체 비용 출력 

    SELECT SUM(replacement_cost)
    FROM film;

- 가장 많은 금액을 사용한 고객 ID를 내림차순별로 출력

    SELECT customer_id,SUM(amount) FROM payment
    GROUP BY customer_id
    ORDER BY SUM(amount) DESC;

- Staff ID가 1과 2인 두 명의 직원이 있습니다. 가장 많은 결제를 처리한 직원에게 보너스를 주려고 한다. 각 직원이 처리한 결제건수는 몇 건이며 누가 보너스를 받는가?

    SELECT staff_id,COUNT(amount)
    FROM payment
    GROUP BY staff_id

- 본사에서 교체 비용과 영화의 MPAA 등급 사이의 관계에 관한 연구를 수행하고 있습니다. MPAA 등급별 평균 교체 비용은 얼마인가?

    SELECT rating, 
    ROUND(AVG(replacement_cost),2)
    FROM film
    GROUP BY rating;

- 총 지출액 또는 총 사용을 기준으로 상위 고객 5명의 고객 ID를 출력

    SELECT customer_id, SUM(amount)
    FROM payment
    GROUP BY customer_id
    ORDER BY SUM(amount) DESC
    LIMIT 5;

- 총 사용 금액이 100달러 이상인 고객ID만 추출

    SELECT customer_id,SUM(amount) FROM payment
    GROUP BY customer_id
    HAVING SUM(amount) > 100;

    - WHERE문으로 GROUP BY문 전에 필터링할 수 있지만 SUM(amount)는 HAVING문으로 필터링 해야한다.

- 충성도가 가장 높은 고객을 위한 플래티넘 서비스를 시작하려 한다. 결제 거래 건수가 40건 이상인 고객에게 플래티넘 지위를 할당하려 한다. 플래티넘 자격이 있는 고객 ID는 누구인가?

    SELECT customer_id,COUNT(*)
    FROM payment
    GROUP BY customer_id
    HAVING COUNT(*) ≥40;

- 직원 ID 2와의 결제 거래에서 100달러를 초과하여 사용한 고객의 ID는 무엇인가?

    SELECT customer_id,SUM(amount)
    FROM payment
    WHERE staff_id = 2
    GROUP BY customer_id
    HAVING SUM(amount)>100;

1. ID가 2인 직원에게서 최소 110달러를 쓴 고객의 ID를 찾으십시오.

    SELECT customer_id,SUM(amount)
    FROM payment
    WHERE staff_id = 2
    GROUP BY customer_id
    HAVING SUM(amount) > 110;

2. J로 시작하는 영화는 몇 편입니까?

    SELECT COUNT(*) FROM film
    WHERE title LIKE 'J%';

3. 이름이 ‘E’로 시작하는 **동시에** 주소 ID가 500 미만인 고객 중, ID 번호가 가장 높은 고객은 누구입니까?

    SELECT first_name,last_name FROM customer
    WHERE first_name LIKE 'E%'
    AND address_id <500
    ORDER BY customer_id DESC
    LIMIT 1;