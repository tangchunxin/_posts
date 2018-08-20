---
title: es搜索 must should
date: 2018-08-17 12:53:44
tags:
---

https://www.cnblogs.com/pilihaotian/p/5830754.html


- 我要查询的组合条件是b=82 || （b=0 && a = 0） || （b=0 && a=100） ，es5.0这个must和should的Java代码怎么写？

```
{"query": {
   "bool": {
     "should": [
       {"bool": {
         "must": [
           {}
         ]
       }
       },
       {"bool": {
         "must": [
           {}
         ]
       }}
     ]
   }
  }
}


```

- must和should查询的组合条件是is_black=2 && （black_type= 10 ||  black_type = 9 || operating_state == 8)

```
2018-08-15 18:20:59::[INFO]    array (
  'index' => 'platform',
  'type' => 'info',
  'sort' => 
  array (
    0 => 'black_time:desc',
  ),
  'size' => 30,
  'from' => 0,
  'body' => 
  array (
    'query' => 
    array (
      'bool' => 
      array (
        'must' => 
        array (
          0 => 
          array (
            'bool' => 
            array (
              'should' => 
              array (
                0 => 
                array (
                  'match' => 
                  array (
                    'black_type' => 10,
                  ),
                ),
                1 => 
                array (
                  'match' => 
                  array (
                    'black_type' => 9,
                  ),
                ),
                2 => 
                array (
                  'match' => 
                  array (
                    'operating_state' => 8,
                  ),
                ),
              ),
            ),
          ),
          1 => 
          array (
            0 => 
            array (
              'term' => 
              array (
                'is_black' => 2,
              ),
            ),
          ),
        ),
      ),
    ),
  ),
)::
2018-08-15 18:20:59::[INFO]    array (
  'index' => 'platform',
  'type' => 'info',
  'sort' => 
  array (
    0 => 'black_time:desc',
  ),
  'size' => 30,
  'from' => 0,
  'body' => 
  array (
    'query' => 
    array (
      'bool' => 
      array (
        'must' => 
        array (
          0 => 
          array (
            'term' => 
            array (
              'is_black' => 1,
            ),
          ),
        ),
        'must_not' => 
        array (
          0 => 
          array (
            0 => 
            array (
              'term' => 
              array (
                'operating_state_combination' => 1,
              ),
            ),
          ),
        ),
      ),
    ),
  ),
)::


```