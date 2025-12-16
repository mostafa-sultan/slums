# Advanced MongoDB Queries

## Introduction

MongoDB provides powerful querying capabilities. This tutorial covers advanced queries, aggregation, and optimization.

---

## Complex Queries

    // Find with multiple conditions
    db.users.find({
      age: { $gte: 18, $lte: 65 },
      city: { $in: ['New York', 'Los Angeles'] }
    });

---

## Aggregation Pipeline

    db.orders.aggregate([
      { $match: { status: 'completed' } },
      { $group: { _id: '$customerId', total: { $sum: '$amount' } } },
      { $sort: { total: -1 } }
    ]);

---

## Conclusion

Master MongoDB queries and aggregation for efficient data retrieval. Use indexes to optimize query performance.

