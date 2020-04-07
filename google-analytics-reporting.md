# How to exclude page starting with a pattern
```php
$dimensionFilter = new Google_Service_AnalyticsReporting_DimensionFilter();
$dimensionFilter->setDimensionName("ga:pagePath");
$dimensionFilter->setOperator("BEGINS_WITH");
$dimensionFilter->setNot(true);
$dimensionFilter->setExpressions(array("/pattern-to-exclude"));

$dimensionFilterClause = new Google_Service_AnalyticsReporting_DimensionFilterClause();
$dimensionFilterClause->setFilters($dimensionFilter);

$request->setDimensionFilterClauses(array($dimensionFilterClause));
```
