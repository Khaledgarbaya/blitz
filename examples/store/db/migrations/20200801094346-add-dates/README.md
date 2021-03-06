# Migration `20200801094346-add-dates`

This migration has been generated by Brandon Bayer at 8/1/2020, 9:43:46 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
PRAGMA foreign_keys=OFF;

CREATE TABLE "quaint"."new_User" (
"createdAt" DATE NOT NULL DEFAULT CURRENT_TIMESTAMP ,"email" TEXT NOT NULL  ,"id" INTEGER NOT NULL  PRIMARY KEY AUTOINCREMENT,"name" TEXT   ,"role" TEXT   ,"storeId" INTEGER   ,"updatedAt" DATE NOT NULL  )

INSERT INTO "quaint"."new_User" ("email", "id", "name", "role", "storeId") SELECT "email", "id", "name", "role", "storeId" FROM "quaint"."User"

PRAGMA foreign_keys=off;
DROP TABLE "quaint"."User";;
PRAGMA foreign_keys=on

ALTER TABLE "quaint"."new_User" RENAME TO "User";

CREATE UNIQUE INDEX "quaint"."User.email" ON "User"("email")

CREATE TABLE "quaint"."new_Product" (
"createdAt" DATE NOT NULL DEFAULT CURRENT_TIMESTAMP ,"description" TEXT   ,"handle" TEXT NOT NULL  ,"id" INTEGER NOT NULL  PRIMARY KEY AUTOINCREMENT,"name" TEXT   ,"price" INTEGER   ,"updatedAt" DATE NOT NULL  )

INSERT INTO "quaint"."new_Product" ("description", "handle", "id", "name", "price") SELECT "description", "handle", "id", "name", "price" FROM "quaint"."Product"

PRAGMA foreign_keys=off;
DROP TABLE "quaint"."Product";;
PRAGMA foreign_keys=on

ALTER TABLE "quaint"."new_Product" RENAME TO "Product";

CREATE UNIQUE INDEX "quaint"."Product.handle" ON "Product"("handle")

PRAGMA "quaint".foreign_key_check;

PRAGMA foreign_keys=ON;
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200429140440-init..20200801094346-add-dates
--- datamodel.dml
+++ datamodel.dml
@@ -2,16 +2,16 @@
 // learn more about it in the docs: https://pris.ly/d/prisma-schema
 datasource sqlite {
   provider = "sqlite"
-  url = "***"
+  url      = "file:./db.sqlite"
 }
 // SQLite is easy to start with, but if you use Postgres in production
 // you should also use it in development with the following:
 //datasource postgresql {
 //  provider = "postgresql"
-//  url = "***"
+//  url      = env("DATABASE_URL")
 //}
 generator client {
   provider = "prisma-client-js"
@@ -21,16 +21,20 @@
 // --------------------------------------
 model User {
   id      Int      @default(autoincrement()) @id
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
   email   String   @unique
   name    String?
   role    String?
   storeId Int?
 }
 model Product {
   id          Int      @default(autoincrement()) @id
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
   handle      String   @unique
   name        String?
   description String?
   price       Int?
```


