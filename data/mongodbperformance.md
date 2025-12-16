# MongoDB Performance Optimization

## Introduction

Optimizing MongoDB performance is crucial for production applications. This tutorial covers indexing, query optimization, and best practices.

---

## Query Optimization

    // Use indexes
    db.users.find({ email: 'user@example.com' }); // Uses index
    
    // Limit results
    db.users.find().limit(10);
    
    // Project only needed fields
    db.users.find({}, { name: 1, email: 1 });

---

## Conclusion

Optimize queries, use indexes, and monitor performance. Use explain() to analyze query execution.

