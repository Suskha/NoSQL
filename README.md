# Result of NoSQL test

***

## Assignment 1

Query

	db.stuff.find({'firstNumber':{'$lt':10000}, 'secondNumber':{'$gt': 5000}}, {'firstNumber':1, 'thirdNumber':1}).sort({'thirdNumber':-1})

Performing manual check on each index with hint and execution statistics on above query.



1.firstNumber_1_secondNumber_1

	db.stuff.find({'firstNumber':{'$lt':10000}, 'secondNumber':{'$gt': 5000}}, {'firstNumber':1, 'thirdNumber':1}).sort({'thirdNumber':-1}).hint('firstNumber_1_secondNumber_1').explain()

*Query Execution Time for Random data: 121ms*
*Query Execution Time for Arranged data: 110ms*

2.firstNumber_1_thirdNumber_1

	db.stuff.find({'firstNumber':{'$lt':10000}, 'secondNumber':{'$gt': 5000}}, {'firstNumber':1, 'thirdNumber':1}).sort({'thirdNumber':-1}).hint('firstNumber_1_thirdNumber_1').explain()
	
*Query Execution Time for Random data : 127ms*
*Query Execution Time for Arranged data: 168ms*

3.thirdNumber_1

	db.stuff.find({'firstNumber':{'$lt':10000}, 'secondNumber':{'$gt': 5000}}, {'firstNumber':1, 'thirdNumber':1}).sort({'thirdNumber':-1}).hint('thirdNumber_1').explain()

*Query Execution Time for Random data : 49ms*
*Query Execution Time for Arranged data: 216ms*

4.firstNumber_1_secondNumber_1_thirdNumber_-1

	db.stuff.find({'firstNumber':{'$lt':10000}, 'secondNumber':{'$gt': 5000}}, {'firstNumber':1, 'thirdNumber':1}).sort({'thirdNumber':-1}).hint('firstNumber_1_secondNumber_1_thirdNumber_-1').explain()

*Query Execution Time for Random data : 109ms*
*Query Execution Time for Arranged data: 101ms*

5._id_

	db.stuff.find({'firstNumber':{'$lt':10000}, 'secondNumber':{'$gt': 5000}}, {'firstNumber':1, 'thirdNumber':1}).sort({'thirdNumber':-1}).hint('_id_').explain()

*Query Execution TIme for Random data : 148ms*
*Query Execution Time for Arranged data: 183ms*


**Answer**

The indexes that could be used by MongoDB to assist in answering query are as follows:

	1.firstNumber_1_secondNumber_1 can be used for find operation.
	2.firstNumber_1_thirdNumber_1 can also be used for find operation.
	3.thirdNumber_1 can be used for sorting.
	4.firstNumber_1_secondNumber_1_thirdNumber_-1 can also be used for find operation.
	5.id cannot be used either in sort or find operation.

## Assignment 2

Options

	1.Add an index on last_name, first_name if one does not already exist. 
	2.Remove all indexes from the collection, leaving only the index on _id in place 
	3.Provide a hint to MongoDB that it should not use an index for the inserts 
	4.Set w=0 on writes 
	5.Build a replica set and insert data into the secondary nodes to free up the primary nodes. 

**Answer**
	
	1.Although adding indexes may improve the read operation, it is not liable because adding more indexes will slow down the insert operation as the indexes need to be updated every time a new record is inserted.

	2.Removing all indexes from the collection, leaving only the index on _id in place is liable beacuse it will improve performance on the insert operation.

	3.Providing a hint to MongoDB that it should not use an index for the inserts is not liable because temporarily disabling indexes is not possible and it has nothing to do with the insert operation.

	4.Setting w=0 on writes is liable because when w=0, there is no waiting and this speeds up the insert operation.

	5.Building a replica set and inserting data into the secondary nodes to free up the primary nodes is not liable because the write opeartion is not possible on the secondary nodes.

## Assignment 3

**Answer : 1**


Reasoning: Once the document is inserted in the collection, we remove that document from animal object and append a new one. Since the append operation is performed on the same document, it shows following message:

	duplicate key error collection: test.animals index: _id_ dup key

resulting in the insertion of a single document only.
