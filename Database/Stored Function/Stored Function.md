```timestamp-url 
https://www.youtube.com/watch?v=I1jjR58Rzic
 ```

사용자 정의 함수
DBMS에 저장되고 사용되는 함수
SQL select, insert, update, delete statement에  사용 가능

#### Stored Function 작성

```sql
delimiter $$ %% [;가 아닌 다른 값] %%
CREATE FUNCTION 함수명( 매개변수)
RETURNS %%자료형%%
%% NO SQL[mySQL 문법]%%
BEGIN
	RETURN 리턴값;
END
delimiter ;
```

```timestamp 
 01:14
 ```
![[Pasted image 20230914103619.png]]

```timestamp 
 04:48
 ```
![[Pasted image 20230914103642.png]]


```timestamp 
 05:21
 ```
```sql
CREATE FUNCTION dept_avg_salary(d_id int)
RETURNS int
%%READS SQL DATA[MYSQL 문법]%%
BEGIN
	%% DECLARE avg_sal int;[DECLARE 변수명 자료형;] @사용시 생략 가능%%
	select avg(salary) into @avg_sal
		from emplooyee
		where dept_id=d_id;
	RETURN @avg_sal;
END
$$

```

```timestamp 
 08:39
 ```


```timestamp 
 09:15
 ```
```sql
CREATE FUNCTION toeic_pass_fail(toeic_score int)
RETURNS char(4)
%%NOSQL mysql 문법%%
BEGIN
	DECLARE pass_fail char(4);
	IF toeic_score is null THEN SET pass_fail = 'fail';
	ELSEIF toeic_score < 800 THEN SET pass_fail = 'fail';
	ELSE SET pass_fail = 'pass';
	END IF;
	RETURN pass_fail;
END
$$
```

```timestamp 
 11:26
 ```
![[Pasted image 20230914104230.png]]

```timestamp 
 11:38
 ```
![[Pasted image 20230914104354.png]]

##### 삭제
-DROP FUNCTION stored_function_name;

##### 등록된 함수 조회
```timestamp 
 12:47
 ```
![[Pasted image 20230914104513.png]]
```timestamp 
 15:05
 ```
![[Pasted image 20230914104732.png]]


