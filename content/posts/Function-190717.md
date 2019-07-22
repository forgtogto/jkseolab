
+++
title = "PostgreSQL Function"
description = ""
type = ["posts","post"]
tags = [
    "postgreSQL",
    "trigger",
    "Function",
]
date = "2019-07-17"
categories = [
    "PostgreSQL",
]
series = ["PostgreSQL"]
[ author ]
  name = "jkseo"
+++

# Trigger Function 190717
- 내가 만든 시퀀스로 1씩 증가하는 펑션 만들기 
~~~
    CREATE OR REPLACE FUNCTION public.wf_issue_number(OUT "number" character varying)
  RETURNS character varying AS
$BODY$
BEGIN 
	number = lpad(nextval('proc_number_seq')::text,9,'0');
END;
$BODY$
  LANGUAGE plpgsql VOLATILE
  COST 100;
ALTER FUNCTION public.wf_issue_number()
  OWNER TO postgres;
COMMENT ON FUNCTION public.wf_issue_number() IS 'TYPE: function';
~~~

- public.wf_issue_number 이라는 이름의 펑션을 만들고
- "number"이라는 값을 리턴 받도록 만든다.
- 실제 sql 문은 $BODY$ 내에 넣게된다.
