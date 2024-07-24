# Filter Query Param Help

> Filter options (`op` = operator):

> - `order_command_id op value`  
> - `side op value`  
> - `created_at op value`  
> - `created_at op value`  
> - `created_at op value ;fee_currency op value`  
> - `created_at op value ;fee_currency op value ;id op value`  
> - `created_at op value`  
> - `created_at op value ;id op value`  
> - `symbol op value`  
> - `symbol op value ;created_at op value`  
> - `symbol op value ;created_at op value ;id op value`  
> - `order_side op value`  
> - `order_side op value ;created_at op value`  
> - `order_side op value ;created_at op value ;id op value`  
> - `order_side op value ;symbol op value`  
> - `order_side op value ;symbol op value ;created_at op value`  
> - `order_side op value ;symbol op value ;created_at op value ;id op value`  
> - `trace_id op value`  
> - `updated_at op value` 

Some of APIs use query params this is how you can use them:

For filtering we use this format:  
`a=value;b>123;c<=456;b#1,2,3`

- Each part contains:
  - a name (name of filtering column)
  - one of `=`, `!=`, `<`, `>`, `<=`, `>=`, `#`
  - a value
- Multiple conditions are separated by `;` and all conditions are `AND`ed together
- We use `#` operate as `in`


 

<aside class="notice">
I hope you can assume that white spaces are just for better readability and in final request you must not use any whitspace between filter name and operator or value
</aside>