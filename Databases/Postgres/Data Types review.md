PostgreSQL can be customized with an arbitrary number of user-defined data types.

varchar(80) specifies a data type that can store arbitrary character strings up to 80 characters in length. 

int is the normal integer type. 

real is a type for storing single precision floating-point numbers.

date should be self-explanatory

int

smallint

real

double precision

char(N)

varchar(N)

date

time

timestamp

interval

point type is an example of a PostgreSQL-specific data type

Constraints: check, not-null, unique, primary keys, foreign keys,

- check constraints: is the most generic constraint type. It allows you to specify that the value in a certain column must satisfy a Boolean (truth-value) expression

CREATE TABLE products (  
    product_no integer,  
    name text,  
    price numeric CHECK (price > 0));

CREATE TABLE products (  
    product_no integer,  
    name text,  
    price numeric CONSTRAINT positive_priceCHECK (price > 0)  
);

- Not-Null constraints: specifies that a column must not assume the null value

CREATE TABLE products (  
    product_no integer NOT NULL,  
    name text NOT NULL,  
    price numeric  
);

- unique constraints: indicates that a column, or group of columns, can be used as a unique identifier for rows in the table. This requires that the values be both unique and not null. 

CREATE TABLE products (  
    product_no integer UNIQUE,  
    name text,  
    price numeric  
);

- primary keys:

CREATE TABLE products (  
    product_no integer PRIMARY KEY,  
    name text,  
    price numeric  
);

CREATE TABLE example (  
    a integer,  
    b integer,  
    c integer,  
    PRIMARY KEY (a, c));

- foreign keys: constraint specifies that the values in a column (or a group of columns) must match the values appearing in some row of another table. We say this maintains the referential integrity between two related tables.

CREATE TABLE products (  
    product_no integer PRIMARY KEY,  
    name text,  
    price numeric  
);

CREATE TABLE orders (  
    order_id integer PRIMARY KEY,  
    product_no integer REFERENCES products (product_no),  
    quantity integer  
);