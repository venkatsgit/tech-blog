
# Unlocking MongoDB Querying for Relational Database Experts

For SQL gurus who have spent years crafting intricate queries and optimizing relational databases, diving into the NoSQL realm can be overwhelming. There are lots of easy guides to help you start using MongoDB, and the MongoDB documentation is known for its clarity. But when you need to make tricky MongoDB queries really fast, this post will show you how.

This post is meant for readers who already know a bit about MongoDB and ChatGPT. If these topics are new to you, you might want to check out the following links before you continue reading this post
- [Query Documents — MongoDB Manual. Query Documents — MongoDB Manual.]( https://www.mongodb.com/docs/manual/tutorial/query-documents/)
- [ChatGPT: OpenAI Platform.]( https://platform.openai.com/docs/models/chatgpt)

## Leveraging ChatGPT for MongoDB Query Assistance

ChatGPT can help you create code for computer programs and write queries for databases. If you need a piece of code or a way to ask a database for specific information, you can describe what you want, and ChatGPT will provide you with examples or solutions to get the job done.

`To ask the right question is already half the solution of a problem. — Carl Jung`

### Key components in the query description

A clear query description should have these important parts

|Field	|Description| Example|
|-------|-----------|--------|
|Collection	|The name of the MongoDB collection you want to query.|orders|
|Criteria|	Conditions that documents must meet (e.g., field-value pairs).|all orders in the 'paid' status with a orderamount greater than $500 and a buyer paid in USD|
|Projections|Fields you want to include in the query result (1 to include, 0 to exclude).|only return the 'ordernumber'  and 'orderamount' fields|
|Sorting|	The order in which query results should be presented (ascending or descending).| sort the results by 'orderamount' in descending order|
|Limit|	The maximum number of documents to include in the result.|limit the output to 10 orders|

### Sample Description and Query Response

Problem Description

    	I have a collection named 'orders' with fields 'ordernumber,' 'status,' 'orderamount,' and 'buyercurrency.' I want to find all order in the 'paid' status with a orderamount greater than $500 and a buyer paid in USD. Please sort the results by 'orderamount' in descending order, limit the output to 10 orders, and only return the 'ordernumber'  and 'orderamount' fields


Query Response from chat gpt

            db.orders.find({
            "status": "paid",
            "orderamount": { $gt: 500 },
            "buyercurrency": "USD"
            }, {
            "ordernumber": 1,
            "orderamount": 1,
            "_id": 0
            }).sort({
            "orderamount": -1
            }).limit(10)

### Ensure proper field names are used 

You can run this script in the MongoDB shell to fetch the field names.

        var sampleDoc = db.order.findOne({});
        function getAllNestedKeys(obj) {
        const keys = [];

        function iterate(obj, currentKey) {
            for (let key in obj) {
            const newKey = currentKey ? `${currentKey}.${key}` : key;
            keys.push(newKey);
            if (typeof obj[key] === 'object') {
                iterate(obj[key], newKey);
            }
            }
        }

        iterate(obj, '');

        return keys;
        }
        var fieldNames = getAllNestedKeys(sampleDoc);
        print(fieldNames);

The previous query has been updated, and the field names have been corrected

        [
            "_id",
            "_id.str",
            "_id.tojson",
            "_id.valueOf",
            "_id.isObjectId",
            "_id.getTimestamp",
            "_id.equals",
            "accountNumber",
            "orderNumber",
            "orderID",
            "orderNumber",
            "orderSoldAmount",
            "orderSoldAmount.amount",
            "orderSoldAmount.currencyCode",
            "orderStatus",
            "timeCreated",
        ]


        db.orders.find({
            "orderStatus": "DELIVERED",
            "orderSoldAmount.amount": { $gt: 500 },
            "orderSoldAmount.currencyCode": "SGD"
        }, {
            "orderNumber": 1,
            "orderSoldAmount.amount": 1,
            "_id": 0
        }).sort({ "orderSoldAmount.amount": -1 }).limit(10);


I haven't discussed JOIN operations in this post because MongoDB encourages embedding related data within a document as the primary approach. JOINs are considered a secondary method and can be achieved through the $lookup stage in the aggregation framework. If you'd like to explore JOIN examples, please leave a comment, and I'll update this post accordingly.