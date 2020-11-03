# 동적procedure 구축법

- - -
node.js 나 다른 여타 백엔드에서 쿼리를 짜고 수행하는 것보다
procedure를 통해 DB에서 쿼리를 수행하는 방식이 속도향상면에서는 좋다.
이때 조건에 따라 쿼리문이 변화되는 상황이 발생하면
정적인 procedure로 데이터를 뽑아내기는 어려운 상황에 놓이게 된다.
이때 동적으로 조건문을 수정할 수 있는 방법을 적는다.
- - -

```javascript
delimiter // 
CREATE PROCEDURE DYNAMIC (IN table VARCHAR(4000), IN col varchar(4000), IN conditions varchar(4000))
BEGIN
    SET @s = CONCAT('SELECT ',col,' FROM ',tbl, ' ', conditions);
    PREPARE stmt FROM @s;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END
//
```
1. 포인트 
- CONCAT으로 넘겨준 파라미터들을 sql 형태로 만들고 PREPARE를 통해 실행시키는 점이 중요하다.
- CRUD를 하나의 procedure로 구현하기 위해 'SELECT' 구문을 아래처럼 변경했는데, procedure에서 없는 테이블 이름이라는 오류를 뱉는다. 이 부분은 조금 더 알아봐야겠다.

```javascript
delimiter // 
CREATE PROCEDURE DYNAMIC (IN crud VARCHAR(4000), IN table VARCHAR(4000), IN col varchar(4000), IN conditions varchar(4000))
BEGIN
    SET @s = CONCAT(crud, col,' FROM ',tbl, ' ', conditions);
    PREPARE stmt FROM @s;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END
//
```

2. 파라미터를 따로 사용할수도 있다.

- 이때 using 키워드를 사용한다.

```javascript
delimiter // 
CREATE PROCEDURE DYNAMIC (IN table VARCHAR(4000), IN col varchar(4000), IN conditions varchar(4000), IN params varchar(4000))
BEGIN
    SET @s = CONCAT('SELECT ',col,' FROM ',tbl, ' ', conditions , '?');
    PREPARE stmt FROM @s;
    EXECUTE stmt USING @params;
    DEALLOCATE PREPARE stmt;
END
//
```

