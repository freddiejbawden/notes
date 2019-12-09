# Revision Questions

# Pig

Q: Explain the difference between MapReduce and Pig

<details>
  <summary>Answer</summary>
  Pig allows users to write SQL-like operations which are compiled down into a series of MapReduce functions. MapReduce does not allow this, each operation must be written by the user; this leads to extra work for the developer. 

  The SQL-like format of Pig make it much better for ad-hoc queries, as it provides a number of predefined functions. 

  Pig functions cannot be optimised for the task at a hand meaning that a Pig query can be slower than one designed by a human 

  Pig provides nested data types which can more accurately and understandably represent data compared to MapReduce.
</details>

Q: What is the difference between a logical and physical plan?

<details>
  <summary>Answer</summary>
  Pig undergoes some steps when a Pig Latin Script is converted into MapReduce jobs by the compiler. Logical and Physical plans are created during the execution of a pig script.

  After performing the basic parsing and semantic checking, the parser produces a logical plan and no data processing takes place during the creation of a logical plan. The logical plan describes the logical operators that have to be executed by Pig during execution. For each line in the Pig script, syntax check is performed for operators and a logical plan is created. If an error is encountered, an exception is thrown and the program execution ends.

  A logical plan contains a collection of operators in the script, but does not contain the edges between the operators.

  After the logical plan is generated, the script execution moves to the physical plan where there is a description about the physical operators, Apache Pig will use, to execute the Pig script. A physical plan is like a series of MapReduce jobs, but the physical plan does not have any reference on how it will be executed in MapReduce
</details>

Q: What are the data types used in Pig? 
<details>
  <summary>Answer</summary>

  There are 4 data types for Pig:

  * **Atom**: a single value 
  * **Tuple**: a series of atoms which may be different data types
  * **Bag**: a collection of tuples which may contain duplicates
  * **Map**: a key value pairing where an atom keys into a bag

</details>

Q: What is a UDF? 
<details>
  <summary>Answer</summary>

  A user defined function which is written in Python/Java, these provide functionality that Pig doesn't natively provide. However they aren't automatically parallelized  

</details>

Q: Suppose we have a table "urls": `(url, category, pagerank)`. Write a Pig query that will find the average pagerank for category with over 100,000 entries.

<details>
  <summary>Answer</summary>

  ```
  page_rank_candidates = FILTER urls by "pagerank" > 0.2
  category_map = COGROUP urls by "category"
  big_categories = FILTER groups BY COUNT(category_map) > 10^6
  output = FOREACH big_categories GENERATE "category", AVG(page_rank_candidates.pagerank)

</details>