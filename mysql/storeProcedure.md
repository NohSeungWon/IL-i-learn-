# storeProcedure 사용법

- - -
아직 명확하지 않지만 
명령의 종료문자를 변환시켜줄 수 있는 DELIMITER를 사용해야 procedure가 생성된다.
그냥 만드려고 하면 procedure안에 종료를 의미하는 ;이 많이 들어가게 되서 
자꾸 sysntax 오류가 발생했다. 
- - -

```javascript

DELIMITER ;;
create procedure test (a int, b int)
begin
select a+b;
end;;
DELIMITER ;

```
