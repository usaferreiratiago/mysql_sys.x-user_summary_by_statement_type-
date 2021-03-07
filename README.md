# mysql_sys.x-user_summary_by_statement_type-

SELECT 
    IF((`performance_schema`.`events_statements_summary_by_user_by_event_name`.`USER` IS NULL),
        'background',
        `performance_schema`.`events_statements_summary_by_user_by_event_name`.`USER`) AS `user`,
    SUBSTRING_INDEX(`performance_schema`.`events_statements_summary_by_user_by_event_name`.`EVENT_NAME`,
            '/',
            -(1)) AS `statement`,
    `performance_schema`.`events_statements_summary_by_user_by_event_name`.`COUNT_STAR` AS `total`,
    `performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_TIMER_WAIT` AS `total_latency`,
    `performance_schema`.`events_statements_summary_by_user_by_event_name`.`MAX_TIMER_WAIT` AS `max_latency`,
    `performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_LOCK_TIME` AS `lock_latency`,
    `performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_ROWS_SENT` AS `rows_sent`,
    `performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_ROWS_EXAMINED` AS `rows_examined`,
    `performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_ROWS_AFFECTED` AS `rows_affected`,
    (`performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_NO_INDEX_USED` + `performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_NO_GOOD_INDEX_USED`) AS `full_scans`
FROM
    `performance_schema`.`events_statements_summary_by_user_by_event_name`
WHERE
    (`performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_TIMER_WAIT` <> 0)
ORDER BY IF((`performance_schema`.`events_statements_summary_by_user_by_event_name`.`USER` IS NULL),
    'background',
    `performance_schema`.`events_statements_summary_by_user_by_event_name`.`USER`) , `performance_schema`.`events_statements_summary_by_user_by_event_name`.`SUM_TIMER_WAIT` DESC
