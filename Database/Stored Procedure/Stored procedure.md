```timestamp-url 
 https://www.youtube.com/watch?v=m2jx18yg8EA&t=2s
 ```

사용자 정의 프로시저
RDB에서 사용
하나의 태스크 수행

두 정수의 덧셈 프로시져
```sql
delimiter $$
CREATE PROCEDURE product(IN a int, IN b int, OUT result int) 
%% IN 키워드 생략 가능 %%
BEGIN
	SET result=a*b;
END
$$
delimiter ;

call product(5, 7, @result);
select @result; %%35%%
```

두 정수 swap
```sql
CREATE PROCEDURE swap(INOUT a int, INOUT b int)
BEGIN
	set @temp = a;
	set a = b;
	set b = @temp;
END
$$

set @a = 5, @b = 7;
call swap(@a, @b);
select @a, @b; %% 7, 5%%
```

부서별 연봉 평균
```sql
CREATE PROCEDURE get_dept_avg_salary()
BEGIN
	select dept_id, avg(salary)
	from employee
	group by dept_id;
END
$$

call get_dept_avg_salary();
```


사용자 이전 닉네임 로그 저장 새 닉네임 업데이트
```sql
CREATE PROCEDURE change_nickname(user_id INT, new_nick varchar(30))
BEGIN
	insert into nickname_logs(
		select id, nickname, now() from users where id= user_id
	);
	update users set nickname = new_nick where id = user_id;
END
$$
```

 