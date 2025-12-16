# MongoDB Aggregation Framework

## Introduction

Aggregation framework processes data and returns computed results. This tutorial covers aggregation stages and operators.

---

## Aggregation Stages

    db.orders.aggregate([
      { $match: { status: 'completed' } },
      { $group: { _id: '$customerId', total: { $sum: '$amount' } } },
      { $project: { customerId: '$_id', total: 1, _id: 0 } },
      { $sort: { total: -1 } },
      { $limit: 10 }
    ]);

---

## Conclusion

Use aggregation for complex data processing. Combine stages to transform and analyze data efficiently.

