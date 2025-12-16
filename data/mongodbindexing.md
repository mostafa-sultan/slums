# MongoDB Indexing

## Introduction

Indexes improve query performance in MongoDB. This tutorial covers creating, managing, and optimizing indexes.

---

## Creating Indexes

    // Single field
    db.users.createIndex({ email: 1 });
    
    // Compound
    db.users.createIndex({ name: 1, age: -1 });
    
    // Unique
    db.users.createIndex({ email: 1 }, { unique: true });

---

## Conclusion

Create indexes on frequently queried fields. Monitor index usage and remove unused indexes.

