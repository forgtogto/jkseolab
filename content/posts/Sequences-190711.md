
+++
title = "PostgreSQL Sequences "
description = ""
type = ["posts","post"]
tags = [
    "postgreSQL",
    "Sequences ",
    "Function",
]
date = "2019-07-11"
categories = [
    "PostgreSQL",
]
series = ["PostgreSQL"]
[ author ]
  name = "jkseo"
+++

##Sequences

postgresql 에서 자동으로 증가값을 넣을때 사용한다.

    CREATE SEQUENCE public.proc_number_seq
      INCREMENT 1
      MINVALUE 1
      MAXVALUE 9223372036854775807
      START 1
      CACHE 1;
    ALTER TABLE public.proc_number_seq
      OWNER TO postgres;

pgadmin3에서 생성할때는 Create script 를 눌러 텍스트로 넣거나  new sequences 를 눌러 기입한다.