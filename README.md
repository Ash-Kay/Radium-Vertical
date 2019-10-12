# Project Radium

This is a **Vertical Slice** of a social networking platform in Node.js

### Tech

Radium uses a number of open source projects to work:
* Knex.js - Knex.js is a SQL query builder.
* Joi - Schema description language and data validator
* Express Async Errors - Handles async errors.
* Express Rate Limit - Limit repeated requests to public APIs and/or endpoints such as password reset
* Jsonwebtoken - for Authentication/ Authorization
* Express - fast node.js network app framework
* MySQL - for database

## Features 
* CRUD posts, comments, users etc.
* Authentication, Authorization
* RateLimit to specific endpoints (or whole api)
* Validation of all input
* Follow MC (of MVC)

## Snippets

Routes
```typescript
router.get("/", UserController.all);
router.post("/signup", validate(schema.userRegister), UserController.signup);
router.post("/login", validate(schema.userLogin), UserController.login);
router.get("/:id", UserController.one);
router.put("/update", verifyAuth, validate(schema.userUpdate), UserController.update);
router.get("/:id/posts", UserController.posts);

export default router;
```

SignUp
```typescript
export const signup = async (request: Request, response: Response, next: NextFunction) => {
    const salt = await bcrypt.genSalt(10);
    const hashed = await bcrypt.hash(request.body.password, salt);
    request.body.password = hashed;

    const user = await db("users").insert(request.body);
    response.status(201).send(user);
};
```

Schema
```sql
CREATE TABLE `users` (
    `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
    `username` VARCHAR(30) NOT NULL UNIQUE,
    `email` VARCHAR(100) NOT NULL UNIQUE,
    `password` VARCHAR(300) NOT NULL,
    `img_url` VARCHAR(100),
    `first_name` VARCHAR(100),
    `last_name` VARCHAR(100),
    `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    `updated_at` TIMESTAMP NULL,
    `last_ip` VARCHAR(100),
    `last_online` TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP,
    `dob` DATE,
    `country` VARCHAR(30) NULL,
    `user_type` varchar(10) NOT NULL DEFAULT 'normal',
    PRIMARY KEY (`id`)
);
```

