# storeProcedure 사용법

- - -
아직 명확하지 않지만 
명령의 종료문자를 변환시켜줄 수 있는 DELIMITER를 사용해야 procedure가 생성된다.
그냥 만드려고 하면 procedure안에 종료를 의미하는 ;이 많이 들어가게 되서 
자꾸 sysntax 오류가 발생했다. 
코드는 아래와 같이 활용한다.
- - -

1. 일반 사용시

```javascript

DELIMITER ;;
CREATE PROCEDURE hello3(a int)
BEGIN
SELECT * FROM test WHERE num = a;
end;;
DELIMITER ;

DELIMITER !!
CREATE PROCEDURE hello3(a INT, b VARCHAR(50)) // varchar의 경우 크기가 지정되지 않으면 오류가 났다.
BEGIN
SELECT * FROM test WHERE num = a;
END!!
DELIMITER ;

DELIMITER ;;
CREATE PROCEDURE list (ins VARCHAR(50))
BEGIN
SELECT * FROM test WHERE test = ins;
END;;
DELIMITER;

```

2. 조건문 사용시

- - -
아래 코드기준으로 
ting parametor가 변할 때
begin 아래 
if (parametor) = '조건' then
  query문;
elseif (parametor) = '조건' then
  query문;
조건 문 더 있을 경우 동일하게 작성
마지막에 조건문이 끝났다는 명렁어를 작성하는 것을 주의하면 된다.
end if;

- - -

```javascript

DELIMITER ;;
CREATE PROCEDURE `test`.getAlllist(
	ting varchar(100)
	)
BEGIN
	IF ting = 'one' then
		SELECT * FROM test;
    elseif ting = 'two' then
    SELECT * FROM test where testColumn = ting ;
	end if;
END; 
;;

DELIMITER ;
```