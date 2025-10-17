# Database Schema

This section provides the SQL Data Definition Language (DDL) for creating the database tables. This schema is designed for PostgreSQL.

```sql
-- Create a custom type for user roles within a store
CREATE TYPE store_role AS ENUM ('owner', 'cashier');

-- Table for Stores
CREATE TABLE stores (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name TEXT NOT NULL,
    address TEXT,
    owner_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE
);
-- Enable Row-Level Security (RLS) for stores
ALTER TABLE stores ENABLE ROW LEVEL SECURITY;
-- Policy: Users can only see stores they are a member of.
CREATE POLICY "Users can see their own stores" ON stores FOR SELECT
USING ( auth.uid() IN (SELECT user_id FROM store_users WHERE store_id = id) );


-- Join table to link Users to Stores with a specific role
CREATE TABLE store_users (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    role store_role NOT NULL,
    UNIQUE(store_id, user_id)
);
-- Enable RLS for store_users
ALTER TABLE store_users ENABLE ROW LEVEL SECURITY;
-- Policy: Users can see their own membership record.
CREATE POLICY "Users can see their own store_user record" ON store_users FOR SELECT
USING ( auth.uid() = user_id );


-- Table for Products
CREATE TABLE products (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    store_id UUID NOT NULL REFERENCES stores(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0),
    quantity INT NOT NULL DEFAULT 0 CHECK (quantity >= 0),
    sku TEXT,
    UNIQUE(store_id, sku)
);
-- Enable RLS for products
ALTER TABLE products ENABLE ROW LEVEL SECURITY;
-- Policy: Users can only see products belonging to a store they are a member of.
CREATE POLICY "Users can see products in their stores" ON products FOR SELECT
USING ( store_id IN (SELECT store_id FROM store_users WHERE user_id = auth.uid()) );
-- Policy: Only owners can insert/update/delete products.
CREATE POLICY "Owners can manage products" ON products FOR ALL
USING ( store_id IN (SELECT store_id FROM store_users WHERE user_id = auth.uid() AND role = 'owner') );
```
