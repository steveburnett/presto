=============
Release 0.287
=============

**Highlights**
==============

**Details**
===========

General Changes
_______________
* Fix a bug in CTE reference node creation, where different CTEs may be incorrectly considered as the same CTE.
* Fix bug with spilling in TopNRowNumber.
* Fix plan canonicalization by canonicalizing plan node ids.
* Fix problem when writing large varchar values to throw a user error when it exceeds internal limits.
* Fix queries that filter with LIKE '%...%' over char columns.
* Fix the regr_count, regr_avgx, regr_avgy, regr_syy, regr_sxx, and regr_sxy functions result to be null when the input data is null, not 0.
* Fixed an issue with heuristic cte materialization strategy where incorrect ctes were materialized.
* Fixes precision loss when timestamp yielded from `from_unixtime(double)` function.
* Improve accuracy and performance of HyperLogLog functions.
* Improve repeat function to create RunLengthEncodedBlock to improve performance.
* Improved latency of materialized ctes by scheduling multiple dependent subgraphs independently.
* Add Heuristic CTE Materialization strategy which auto materialized expensive ctes. This is configurable by setting ``cte_materialization_strategy`` to ``HEURISTIC`` or ``HEURISTIC_COMPLEX_QUERIES_ONLY``. (default ``NONE``).
* Add Prestissimo Developer Guide topic to the Presto documentation.
* Add a config `default-view-security-mode` to choose the default security mode for view creation.
* Add a new plan canonicalization strategy `ignore_scan_constants` which canonicalize predicates for both partitioned and non-partitioned columns in scan node.
* Add a session property `track_history_based_plan_statistics_from_complete_stages_in_failed_query` to enable tracking hbo statistics from complete stages in failed queries.
* Add an optimizer rule to get rid of map cast in map access functions when possible.
* Add configuration property `legacy_json_cast` whose default value is `true`. See [Properties Reference](http://prestodb.io/docs/current/admin/properties.html#legacy-compatible-properties).
* Add documentation for supported data types mapping in the Iceberg connector.
* Add histogram column statistic to Presto for the optimizer. Connectors can now implement support for them.
* Add limit to the amount of data written during CTE Materialization. This is configurable by the session property ``query_max_written_intermediate_bytes`` (default is 2TB).
* Add log of stats equivalent plan and canonicalized plan for HBO. This feature is controlled by session property `log_query_plans_used_in_history_based_optimizer`.
* Add optimization for query plans which contains RowNumber and TopNRowNumber nodes with empty input.
* Add session property `history_optimization_plan_canonicalize_strategy` to specify the plan canonicalization strategies to use for HBO.
* Add support for Iceberg connector in presto_protocol.
* Add support for tracking of the input data size even when there is a fragment result cache hit. This can be enabled by setting the config ```fragment-result-cache.input-data-stats-enabled=true```.
* Add usage documentation for :doc:`/clients/presto-cli`.
* Add usage documentation for :doc:`/clients/presto-console`.
* Add worker type and query ID information in HBO stats.
* Remove native_execution_enabled, native_execution_executable_path and native_execution_program_arguments session properties. These are no longer used. Corresponding configuration properties are still available.
* Remove the configuration property ``use-legacy-scheduler`` and the corresponding session property ``use_legacy_scheduler`.   The property previously defaulted to true, and the new scheduler, which was intended to replace it eventually, was never productionized and is no longer needed. The configuration property ``max-stage-retries`` and the session property ``max_stage_retries`` have also been removed.
* CAST(str as INTEGER), CAST(str as BIGINT), CAST(str as SMALLINT), CAST(str as TINYINT) now allow leading and trailing spaces in the string.
* Deprecate SPI method ConnectorMetadata.getTableLayouts(), replace by ConnectorMetadata.getTableLayoutForConstraint().
* Enable propagation of logical properties by default.
* Enable propagation of logical properties by default.
* Introduce Iceberg Connector in Prestissimo.
* JSON is now a supported output format in the Presto CLI.
* Move `SortNode` to SPI module to be utilized in connector.
* New support for Apache DataSketches KLL sketch with the sketch_kll and related family of functions.
* Optimized `map_normalize` builtin SQL UDF to avoid repeated reduce computation.
* Upgrade Alluxio to 310.
* Upgrade Alluxio to 312.

Security Changes
________________
* Remove logback 1.2.3.

Iceberg Connector Changes
_________________________
* Fixes: Fix error encountered when attempting to execute an INSERT INTO statement, in cases where column names contain white spaces.
* Added support for row-level deletes on Iceberg V2 tables. The delete mode can be changed from ``merge-on-read`` to ``copy-on-write`` by setting table property ``delete_mode``.

Verifier Changes
________________
* Add support to do extended bucket verification for INSERT and CreateTableAsSelect queries. This can be enabled by the configuration property ``extended-verification``. It would verify each bucket's data checksum if the inserted table is bucketed.
* Add support to do extended partition verification for INSERT and CreateTableAsSelect queries. This can be enabled by the configuration property ``extended-verification``. It would verify each partition's data checksum if the inserted table is partitioned.

SPI Changes
___________
* Com.facebook.common.Page has a new replaceColumn method.

Hive Changes
____________
* Fix a potential wrong results bug when footer stats are marked unreliable and partial aggregation pushdown is enabled.  Such queries will now fail with an error.
* Improves the `hive.orc.use-column-names` configuration setting to no longer fail on reading ORC files without column names but falls back to using Hive's schema, enhancing compatibility with legacy ORC files.
* Add session property ``hive.dynamic_split_sizes_enabled`` to use dynamic split sizes based on data selected by query.
* Add support for Filelist caching for symlink tables.
* Added quick stats, a mechanism to build stats from metadata for tables/partitions that are missing stats.
* $row_id is a new hidden column.
* Introduce system procedure to invalidate directory list cache in Hive Catalog.

Iceberg Changes
_______________
* Fix identity and truncate transforms on DecimalType columns.
* Fix the bug that `CAST` from non-legacy timestamp to date rounding to future when the timestamp is prior than `1970-01-01 00:00:00.000`.
* Add the support to set commit.retry.num-retries table property with table creation to make the number of attempts to make in case of concurrent upserts configurable.
* Support year/month/day/hour transforms both on legacy and non-legacy TimestampType column.

Mysql Changes
_____________
* Support timestamp column type.

Prestissimo (native Execution) Changes
______________________________________
* Add support to read Iceberg V2 tables with Position Deletes.

**Credits**
===========

8dukongjian, Ajay George, Amit Dutta, Anant Aneja, Andrii Rosa, Athmaja N, Avinash Jain, Bikramjeet Vig, Christian Zentgraf, Deepa George, Deepak Majeti, Eduard Tudenhoefner, Elliotte Rusty Harold, Emanuel F, Fazal Majid, Jalpreet Singh Nanda (:imjalpreet), Jialiang Tan, Jimmy Lu, Jonathan Hehir, Karteekmurthys, Ke, Kevin Wilfong, Konjac Huang, Lyublena Antova, Masha Basmanova, Mohan Dhar, Nikhil Collooru, Pranjal Shankhdhar, Pratik Joseph Dabre, Rebecca Schlussel, Reetika Agrawal, Rohit Jain, Sanika Babtiwale, Sergey Pershin, Sergii Druzkin, Sreeni Viswanadha, Steve Burnett, Sudheesh, Swapnil Tailor, Tai Le Manh, Timothy Meehan, Todd Gao, Vivek, Will, Yihong Wang, Ying, Zac Blanco, Zac Wen, Zhenxiao Luo, aditi-pandit, dnskr, feilong-liu, hainenber, ico01, jaystarshot, kedia,Akanksha, kiersten-stokes, polaris6, pratyakshsharma, s-akhtar-baig, sabbasani, wangd, wypb, xiaodou, xiaoxmeng
