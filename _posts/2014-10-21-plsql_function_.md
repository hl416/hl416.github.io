---
layout: post
title: plsql function 
description: ""
modified: 2014-10-21
category: articles
tags: [life, love]
---

CREATE OR REPLACE FUNCTION functest(start_date text, end_date text)
RETURNS table(m integer, y integer, H integer, S integer, L integer)
AS $$
BEGIN
RETURN QUERY
SELECT x::integer,x,x,x,x from locations limit 10;
END;
$$ LANGUAGE plpgsql;


the naming problems
