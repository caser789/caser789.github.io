---
layout: post
title: Airflow model
date:   2017-08-02 16:18:01 +0800
categories: airflow model
tag: airflow
---

* content
{:toc}


## 1. connection

key                | value
-----              | -----
id                 | 19
conn_id            | aws_default
conn_type          | aws
host               | NULL
schema             | NULL
login              | NULL
password           | NULL
port               | NULL
extra              | AAAAB
is_entrypted       | 0
is_extra_encrypted | 1

## 2. dag

key                | value
-----              | -----
dag_id             | example_bash_operator
is_paused          | 1
is_subdag          | 0
is_active          | 1
last_scheduler_run | 2017-08-02 03:55:57.906440
last_pickled       | NULL
last_expired       | NULL
scheduler_lock     | NULL
pickle_id          | NULL
fileloc            | /opt/service/airflow/1.0.0/local/lib/python2.7/site-packages/airflow/example_dags/example_bash_operator.py
owners             | airflow

## 3. dag_run

key              | value
-----            | -----
id               | 1
dag_id           | example_xcom
execution_date   | 2017-08-02 03:56:57.659926
state            | running
run_id           | manual__2017-08-02T03:56:57.659926
external_trigger | 1
conf             | }q.
end_date         | NULL
start_date       | 2017-08-02 04:03:27.086868

## 4. dag_stats

key    | value
-----  | -----
dag_id | example_xcom
state  | running
count  | 1
dirty  | 0

## 5. job

key              | value
-----            | -----
id               | 1
dag_id           | NULL
state            | running
job_type         | SchedulerJob
start_date       | 2017-08-02 03:56:17.651430
end_date         | NULL
latest_heartbeat | 2017-08-02 03:59:28.379003
executor_class   | LocalExecutor
hostname         | ip-192-168-1-103.ec2.internal
unixname         | ubuntu

## 6. known_event_type

key             | value
-----           | -----
id              | 1
know_event_type | Holiday

## 7. log

key            | value
-----          | -----
id             | 3
dttm           | 2017-08-02 03:57:29.016977
dag_id         | example_xcom
task_id        | NULL
event          | graph
execution_date | 2017-08-02 03:56:57.659926
owner          | NULL
extra          | [('execution_date', u'2017-08-02 03:56:57.659926'), ('dag_id', u'example_xcom')]

## 8. xcom
## 9. slot_pool
## 10. task_fail
## 11. task_instance
## 12. users
## 13. variable
## 14. dag_pickle
## 15. import_error
## 16. known_event
## 17. sla_miss


## 18. alembic_version

+--------------+
| version_num  |
+--------------+
| 127d2bf2dfa7 |
+--------------+

## 19. chart

key                 | value
-----               | -----
id                  | 1
label               | Airflow task instance by type
conn_id             | airflow_db
user_id             | NULL
chart_type          | bar
sql_layout          | series
sql                 | SELECT state, COUNT(1) as number FROM task_instance WHERE dag_id LIKE 'example%' GROUP BY state
y_log_scale         | NULL
show_datatable      | NULL
shwo_sql            | 1
height              | 600
defautl_params      | {}
x_is_date           | 0
iteration_no        | 0
last_modified       | 2017-08-02 03:55:58



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
