# MongoDB Relationships

## Introduction

MongoDB supports relationships through embedded documents and references. This tutorial covers both approaches.

---

## Embedded Documents

    db.users.insertOne({
      name: 'John',
      address: {
        street: '123 Main St',
        city: 'New York'
      }
    });

---

## References

    // Users
    db.users.insertOne({ _id: 1, name: 'John' });
    
    // Orders
    db.orders.insertOne({ userId: 1, product: 'Laptop' });
    
    // Join with $lookup
    db.orders.aggregate([
      { $lookup: {
        from: 'users',
        localField: 'userId',
        foreignField: '_id',
        as: 'user'
      }}
    ]);

---

## Conclusion

Use embedded documents for one-to-one or one-to-few relationships. Use references for one-to-many relationships.

