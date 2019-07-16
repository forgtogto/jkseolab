
+++
title = "PostgreSQL Trigger Function"
description = ""
type = ["posts","post"]
tags = [
    "postgreSQL",
    "trigger",
    "Function",
]
date = "2019-04-02"
categories = [
    "PostgreSQL",
]
series = ["PostgreSQL"]
[ author ]
  name = "jkseo"
+++

# Trigger Function 190716
- Cmdbuild 내 트리거 펑션을 통하여 값을 자동으로 넣어주게 하는데 목적이 있따.
~~~
    -- Function : pubilc.tt_floor_descr()
    -- DROP FUNCTION public.tt_floor_descr();
    CREATE OR REPLACE FUNCTION public.tt_floor_descr()
    	RETURNS trigger AS
    $BODY$
    DECLARE
    	building VARCHAR;
    	level_code VARCHAR;
    BEGIN
    		SELECT concat_ws( ' ', "Code", "Description" ) INTO building
    		FROM "Building"
    		WHERE "Id" = NEW."Building";

    		SELECT "Code" INTO level_code
    		FROM "LookUp"
    		WHERE "Id" = NEW."Level";

    		NEW."Description" = concat_ws(' - ', building, level_code );
    		RETURN NEW;
    END;
    $BODY$
      LANGUAGE plpgsql VOLATILE
      COST 100;
    ALTER FUNCTION public.tt_floor_descr()
      OWNER TO postgres;
~~~

### CREATE OR REPLACE

오브젝트를 생성하거나 수정한다.

### FUNCTION public.tt_floor_descr() RETURNS trigger AS

펑션의 이름이고 트리거를 반환한다 .

### declare

사용할 변수를 선언하는 곳  여기서는 빌딩과 레벨 코드를 선언함

### $BODY$

참고사항

[https://sas-study.tistory.com/182](https://sas-study.tistory.com/182)

> - create or replace : 오브젝트를 생성하거나 수정하겠다는 의미. 처음생성할땐 create명령어가 먹고 drop 하지 않는 한 replace가 그다음부터는 실행될것이다. 따라서 대부분 오브젝트를 생성할때 생성하거나 수정한다~ 라는 키워드로 같이 쓰는편.
- function hate_count() returns trigger as $trigger_hate_count$ : 함수 hate_count()를 생성할건데 이 함수는 트리거를 반환한다. 그 트리거이름을 $$ 사이에 넣어준다.
- declare : 사용할 변수선언하는 곳, 여기서는 count값이 필요해서 선언해두었다.
- begin ~ end : 몸통부분, 표현하고자 하는 일련의 과정을 적는 곳.
- TG_OP : 발생하는 트리거 이벤트 종류, INSERT, DELETE, UPDATE, TRUNCATE가 있다고 한다. 본인은 INSERT와 DELETE가 필요하기 떄문에 두개만 해당.
- new. / old. : insert나 delete update 이벤트를 발생시킬때 사용하는 접근변수? 라고한다. insert/update 이벤트의 경우 new 키워드로 새로 들어오는 값에 접근할 수 있고, delete / update 이벤트의 경우는 old 키워드로 제거/수정되고 난후의 값에 접근할 수 있다. 접근할 수 있는 값은 트리거가 바라보고 있는 테이블에 한해서 가능하다.
- into : select 문을 이용해서 어떤 단일값을 변수 count에 담을때 저러한 문법으로 담아야 한다.
- for each row : 각각의 row에 접근할 수 있다. 이걸 해야 .new / .old 키워드로 삽입/삭제/수정되는 값들에 접근할 수 있다.
- raise notice '문자열 %',변수 : 일반 출력문으로 count값과 old / new 값들이 잘 나오는지 확인하기 위해서 디버깅용으로 사용했던 것들, % 갯수만큼 뒤에 출력할 변수들을 덧붙여 주면된다.