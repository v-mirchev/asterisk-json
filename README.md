An Asterisk macro definition for parsing JSON strings and fetching values from tree nodes. Inline PHP used, though it can be any other language. Solved Asterisk and bash escaping problem.

Example:
```json
{
  "success": true,
  "result": {
    "items": [
      {
        "name": "name of item 1",
        "value": "value of item 1",
      },
      {
        "name": "name of item 2",
        "value": "value of item 2",
      },
      {
        "name": "name of item 3",
        "value": "value of item 3",
      }
    ]
  }
}

```

```
  ...
  same => n,SET(CURL_RESPONSE=${CURL(http://api.com/json)})
  same => n,Macro(json,/result/items/2/name,CURL_RESPONSE,SECOND_ITEM_NAME)
  same => n,Noop(${SECOND_ITEM_NAME})
  same => n,Noop(${MACRO_JSON_RESULT})
  same => n,Noop(${MACRO_JSON_ERROR_MESSAGE})

```


"Unminified" PHP code (Asterisk variables injected):
```php
$paths = explode('/', trim('${ARG1}', '/')); 

!empty($paths) OR die('ERROR:MISSING_PATH'); 
!empty($argv[1]) OR die('ERROR:EMPTY_JSON_STRING'); 
$json = json_decode($argv[1], true) OR die('ERROR:INVALID_JSON_STRING_FORMAT ' . $json); 

foreach ($paths as $path) {
  $json = isset($json[$path]) ? $json[$path] : die('ERROR:PATH_NOT_FOUND ' . $path); 
}
  
echo 'SUCCESS:' . $json;
```
