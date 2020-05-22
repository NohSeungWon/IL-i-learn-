# storeProcedure 사용법

- - -
아직 명확하지 않지만 
명령의 종료문자를 변환시켜줄 수 있는 DELIMITER를 사용해야 procedure가 생성된다.
그냥 만드려고 하면 procedure안에 종료를 의미하는 ;이 많이 들어가게 되서 
자꾸 sysntax 오류가 발생했다. 
- - -

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

```
