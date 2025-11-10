[https://www.postgresql.org/docs/9.1/functions.html](https://www.postgresql.org/docs/9.1/functions.html)

|   |   |   |
|---|---|---|
|to_tsvector|returns a tsvector for the text or document passed as an argument, which can be used to perform efficient text searches.|select id, title<br><br>from core.product<br><br>where to_tsvector(title) @@ to_tsquery('ног');|
|nextval|supplies successive values from a sequence object|CREATE TABLE products (product_no integer DEFAULT nextval('products_product_no_seq'));|
||||
||||

pg notify/listen