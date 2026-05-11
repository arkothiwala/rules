This repository contains important rules / PR checklist.

# Type hinting
- Adding `Any` from typings is equivalent to not adding any typehint. Typehints have to be explicit i.e. nullable or multiple with Union

# FastAPI
- Type hints - Always declare type hinting in FastAPI. Pydantic serializes response objects in Rust if you declare a return type of the path operation function.
- If you aren't using outputs of the dependency function in the path operation function, then better declare it in the decorator instead of function arguments. This helps avoid tooling/editor error for unused variables or avoid confusion to the new developers.

# PySpark
- from pyspark.sql.functions import *
    - This is banned because it has min, max and other functions that conflict with native python functions and it leads to pipeline failures with sometimes error so unclear that you can't trace it back.
- avoid outer joins and cross joins as much as possible -> it blows things up.
- Avoid for loops in pyspark code
- Replace multiple `WithColumn` statements with a single `WithColumns` statement
    - This is because `WithColumn` creates a new DataFrame object that wraps the previous one.
    - This results in a linked list of logical plans. If you chain 100 .withColumn() calls, Spark has to traverse a logical plan that is 100 levels deep.
        - This increases driver's planning time and increases optimization overhead for the catalyst optimizer
- Often when a join is talking really long, the data has duplicates on the join key -> that is blowing up the pipeline
