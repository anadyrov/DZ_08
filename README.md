# DZ_08
-- Описание/Пошаговая инструкция выполнения домашнего задания:
-- • развернуть виртуальную машину любым удобным способом
Выполнено
NAME: postgres25032022
ZONE: us-central1-a
MACHINE_TYPE: e2-medium
PREEMPTIBLE:
INTERNAL_IP: 10.128.0.14
EXTERNAL_IP: 34.71.183.232
STATUS: RUNNING

-- • поставить на неё PostgreSQL 14 из пакетов собираемых postgres.org
Выполнено

-- • настроить кластер PostgreSQL 14 на максимальную производительность не обращая внимание на возможные проблемы с надежностью в случае аварийной перезагрузки виртуальной машины
-- В конфигурационном файле прописал новые параметры для кластера
/etc/postgresql/14/main/postgresql.conf
include_dir = 'conf.d'

-- • нагрузить кластер через утилиту
-- https://github.com/Percona-Lab/sysbench-tpcc (требует установки https://github.com/akopytov/sysbench) или через утилиту pgbench (https://postgrespro.ru/docs/postgrespro/14/pgbench)

pgbench -i laba08
pgbench -c 50 -j 2 -P 10 -T 60 laba08

-- • написать какого значения tps удалось достичь, показать какие параметры в какие значения устанавливали и почему
-- Исходя из технических характеристик тестовой VM (RAM 3GB, CPU 2, ssd) выставил следующие параметры:
max_connections = 200  -- Если выставить меньше то утилита pgbench ругалась "FATAL:  sorry, too many clients already"
shared_buffers = 768MB
effective_cache_size = 2304MB
maintenance_work_mem = 192MB
checkpoint_completion_target = 0.9
wal_buffers = 16MB
default_statistics_target = 100
random_page_cost = 1.1
effective_io_concurrency = 200
work_mem = 39321kB
min_wal_size = 2GB
max_wal_size = 8GB
max_worker_processes = 2
max_parallel_workers_per_gather = 1
max_parallel_workers = 2
max_parallel_maintenance_workers = 1

-- Результат получился такой:
starting vacuum...end.
progress: 10.0 s, 655.3 tps, lat 74.846 ms stddev 55.036
progress: 20.0 s, 691.9 tps, lat 72.290 ms stddev 51.457
progress: 30.0 s, 700.9 tps, lat 71.365 ms stddev 53.184
progress: 40.0 s, 711.6 tps, lat 70.191 ms stddev 51.070
progress: 50.0 s, 710.8 tps, lat 70.333 ms stddev 50.930
progress: 60.0 s, 673.5 tps, lat 74.325 ms stddev 57.708



-- На работе для системы (отчетность) удалось увеличить производительность базы на ~ 30% благодаря следующим параметрам:
max_connections = 100
shared_buffers = 2GB
effective_cache_size = 6GB
maintenance_work_mem = 1GB
checkpoint_completion_target = 0.9
wal_buffers = 16MB
default_statistics_target = 500
random_page_cost = 4
effective_io_concurrency = 2
work_mem = 1048kB
min_wal_size = 4GB
max_wal_size = 16GB
max_worker_processes = 2
max_parallel_workers_per_gather = 1
max_parallel_workers = 2
max_parallel_maintenance_workers = 1











