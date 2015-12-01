---
layout: post
title: Postgres Performance Tuning
description: ""
modified: 2015-02-01
category: articles
tags: [life, love]
---

B-tree,
B+tree
why B+tree is good for index. cos the 4kb page can hold more index. 

index,partition
explain analyze query;
explain table;
function: dynamic query;
data type: macaddr, int, use as little space as possible.
sometimes dot not need to sort, this can save a lot of time.
index on date but not on timestmap
partial index

index sometimes can even worse, the engine tries to guess the optimal query plan, but might be wrong.

disk performance:
-bash-4.1$ dd if=16706 of=~/speetest bs=1K count=100000
52472+0 records in
52472+0 records out
53731328 bytes (54 MB) copied, 0.222327 s, 242 MB/s

view all activities:
SELECT datname,procpid,current_query FROM pg_stat_activity

when group by, the query plan might be different depending on whether there is enough memory: 1) sorting and then aggregate 2) hashing and then aggregate

setting a larger work_item memory to avoid external sorting.  or work_mem?

partition may not speed up the performance

get_user_path_to_replay: only qtime index: 900ms, qtime index & mac index: 5000ms, combined index on qtime and mac: 90ms
