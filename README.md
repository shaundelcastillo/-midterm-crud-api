Reflection Answers
1. What was the most challenging part of building this API?
The most challenging part was configuring runtime TypeScript execution and decorator metadata reflection. Managing how tsx and TypeORM parse entity column definitions without throwing type errors, alongside aligning local database port connections, required precise environment and compiler alignment.

2. How does TypeORM make database operations easier compared to raw SQL?
TypeORM abstracts SQL queries into object-oriented TypeScript classes and repository methods (e.g., userRepository.find(), save(), remove()). This eliminates manual string concatenation of SQL queries, prevents syntax errors, provides strong typing, and makes database maintenance significantly more intuitive.

3. Why is it important to exclude the password field from API responses? How did you implement this?
Excluding passwords prevents sensitive user data from being exposed in HTTP responses or intercepted by malicious actors. We implemented this securely in TypeORM by setting select: false directly on the password column decorator inside the User entity, ensuring it is never retrieved by default queries unless explicitly requested.

4. Describe a scenario where you would need to use database transactions instead of individual operations.
A prime scenario is an e-commerce checkout or bank transfer where multiple dependent writes occur across tables (e.g., debiting an account balance while simultaneously recording a transaction history log). If one operation fails, the entire transaction must roll back to prevent data corruption or partial state inconsistencies.

5. What is the difference between synchronize: true and migrations? When should you use each?
synchronize: true: Automatically alters and syncs the database schema to match your entity classes on every application boot. Use this only in development for rapid prototyping.

Migrations: Provide a controlled, version-controlled history of schema changes via explicit migration scripts. Use this in production to prevent accidental data loss and safely manage schema updates.

6. If you had to add a Product entity with a relationship to User (a user can have many products), how would you modify the code?
We would create a Product.ts entity file containing a @ManyToOne(() => User, (user) => user.products, { onDelete: 'CASCADE' }) decorator on the user property, and add a corresponding @OneToMany(() => Product, (product) => product.user) array property inside the User entity to establish the inverse relation.

7. What security improvements would you make before deploying this API to production?
Implement secure password hashing using bcrypt before saving credentials to the database.

Add rate-limiting middleware to prevent DDoS and brute-force attacks.

Implement JWT (JSON Web Token) authentication and role-based authorization for protected routes.

Turn off database synchronization (synchronize: false) and enable strict environment variable validation.