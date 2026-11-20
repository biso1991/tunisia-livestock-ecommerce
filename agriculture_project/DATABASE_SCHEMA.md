# 🗄️ Database Schema - Agriculture Livestock E-commerce

## Entity Relationship Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              USER (Django Auth)                              │
│  • id (UUID)                                                                 │
│  • username                                                                  │
│  • email                                                                     │
│  • password                                                                  │
│  • first_name, last_name                                                    │
│  • is_active, is_staff, is_superuser                                        │
└──────────────┬──────────────────────────────────────┬───────────────────────┘
               │                                      │
               │ owner (FK)                           │ owner (FK)
               │                                      │
               ▼                                      ▼
┌──────────────────────────────────┐   ┌─────────────────────────────────────┐
│           MARKET                 │   │          PRODUCT (Livestock)         │
│                                  │   │                                      │
│  • id (AutoIncrement)            │   │  IDENTIFICATION:                     │
│  • uuid (UUID)                   │◄──┤  • id (AutoIncrement)                │
│  • name                          │   │  • uuid (UUID)                       │
│  • mark_number (Enum)            │   │  • product_name (200 chars)         │
│  • owner (FK → User)             │   │  • description (TextField)          │
│  • created_at                    │   │                                      │
│  • updated_at                    │   │  LIVESTOCK ATTRIBUTES:               │
│                                  │   │  • breed (Enum)                     │
│  Choices:                        │   │    - تبار (Tabar)                   │
│    - product_one                 │   │    - غربي (Gharbi)                  │
│    - product_two                 │   │    - سردي (Sardi)                   │
│    - product_three               │   │    - سويسري (Swiss)                 │
│    - product_four                │   │    - مختلط (Mixed)                  │
│                                  │   │    - أخرى (Other)                   │
└──────────────────────────────────┘   │  • gender (Enum)                    │
                                       │    - ذكر (Male)                      │
                  market_k (FK)        │    - أنثى (Female)                   │
                         │             │    - مختلط (Mixed)                   │
                         └─────────────┤  • age_months (Integer)             │
                                       │  • weight_kg (Decimal 6,2)          │
                                       │  • quantity_available (Integer)     │
                                       │                                      │
                                       │  LOCATION (Tunisia):                │
                                       │  • governorate (Enum - 24 options)  │
                                       │    - باجة (Béja)                    │
                                       │    - تونس (Tunis)                   │
                                       │    - أريانة (Ariana)                │
                                       │    - ... (21 more)                  │
                                       │  • city (100 chars)                 │
                                       │    e.g., "مجاز الباب"               │
                                       │                                      │
                                       │  PRICING:                           │
                                       │  • price (Decimal 10,3 TND)         │
                                       │  • wholesale_price (Decimal 10,3)   │
                                       │  • min_wholesale_quantity (Int)     │
                                       │  • stock (Integer)                  │
                                       │                                      │
                                       │  MEDIA:                             │
                                       │  • image_main (ImageField)          │
                                       │  • image_2 (ImageField)             │
                                       │  • image_3 (ImageField)             │
                                       │  • image_4 (ImageField)             │
                                       │  • video (FileField)                │
                                       │                                      │
                                       │  BUSINESS:                          │
                                       │  • is_featured (Boolean)            │
                                       │  • is_active (Boolean)              │
                                       │  • views_count (Integer)            │
                                       │  • rating (Float 0.0-5.0)           │
                                       │  • type (Enum)                      │
                                       │  • Buy_sell (Enum)                  │
                                       │                                      │
                                       │  SOCIAL MEDIA:                      │
                                       │  • facebook_post_id (100 chars)     │
                                       │  • instagram_post_id (100 chars)    │
                                       │  • tiktok_post_id (100 chars)       │
                                       │                                      │
                                       │  RELATIONSHIPS:                     │
                                       │  • owner (FK → User)                │
                                       │  • market_k (FK → Market)           │
                                       │                                      │
                                       │  TIMESTAMPS:                        │
                                       │  • created_at (DateTime)            │
                                       │  • updated_at (DateTime)            │
                                       │                                      │
                                       │  INDEXES:                           │
                                       │  • idx_created_at (DESC)            │
                                       │  • idx_location (governorate+breed) │
                                       │  • idx_active (is_active+featured)  │
                                       └──────────┬───────────────────────────┘
                                                  │
                                                  │ product (FK)
                                                  │
                                                  ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                               ORDER_ITEM                                     │
│  • id (AutoIncrement)                                                        │
│  • uuid (UUID)                                                              │
│  • product (FK → Product)                                                   │
│  • quantity (Integer)                                                       │
│  • price (Decimal 10,2)                                                     │
│  • owner (FK → User)                                                        │
│  • item_K (FK → Order)                                                      │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │
                            item_K (FK)│
                                       │
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                                ORDER                                         │
│  • id (AutoIncrement)                                                        │
│  • order_date (DateTime)                                                    │
│  • status (Enum)                                                            │
│    - idle                                                                   │
│    - pending                                                                │
│    - delivered                                                              │
│    - canceled                                                               │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │
                                       │ order (FK)
                                       │
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                               PAYMENT                                        │
│  • id (AutoIncrement)                                                        │
│  • uuid (UUID)                                                              │
│  • payment_method (Enum)                                                    │
│    - cash (Cash on Delivery - Primary for Tunisia)                         │
│    - Rip (Bank Transfer)                                                   │
│  • singel_amount (Decimal 10,2)                                            │
│  • group_amount (Decimal 10,2)                                             │
│  • owner (FK → User)                                                        │
│  • order (FK → Order)                                                       │
│  • stock_k (FK → Product)                                                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 📊 Table Specifications

### 1. **PRODUCT** (Main Table - Livestock)

**Table Name:** `gr_product`  
**Purpose:** Store livestock (sheep) listings with full details

#### Field Groups:

##### A. Identification (5 fields)
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | AutoIncrement | PK | Auto-generated ID |
| uuid | UUID | Unique, Indexed | Public identifier |
| product_name | CharField(200) | Not Null | Arabic/French name |
| description | TextField | Nullable | Detailed description |
| created_at | DateTime | Auto | Listing creation time |
| updated_at | DateTime | Auto | Last modification |

##### B. Livestock Attributes (5 fields) ⭐ NEW
| Field | Type | Choices | Description |
|-------|------|---------|-------------|
| breed | CharField(50) | تبار, غربي, سردي, سويسري, مختلط, أخرى | Sheep breed |
| gender | CharField(20) | ذكر, أنثى, مختلط | Male/Female/Mixed |
| age_months | Integer | ≥ 0 | Age in months |
| weight_kg | Decimal(6,2) | ≥ 0 | Weight in kilograms |
| quantity_available | Integer | ≥ 1 | Number of animals |

##### C. Location (2 fields) ⭐ NEW
| Field | Type | Choices | Description |
|-------|------|---------|-------------|
| governorate | CharField(50) | 24 Tunisian governorates | باجة, تونس, أريانة, etc. |
| city | CharField(100) | Free text | مجاز الباب, etc. |

##### D. Pricing (4 fields)
| Field | Type | Default | Description |
|-------|------|---------|-------------|
| price | Decimal(10,3) | 0 | Regular price (TND) |
| wholesale_price | Decimal(10,3) | 0 | Bulk price (TND) |
| min_wholesale_quantity | Integer | 5 | Minimum for wholesale |
| stock | Integer | 0 | Legacy stock field |

##### E. Media (5 fields) ⭐ NEW
| Field | Type | Path | Description |
|-------|------|------|-------------|
| image_main | ImageField | livestock/images/%Y/%m/%d/ | Primary photo |
| image_2 | ImageField | livestock/images/%Y/%m/%d/ | Second photo |
| image_3 | ImageField | livestock/images/%Y/%m/%d/ | Third photo |
| image_4 | ImageField | livestock/images/%Y/%m/%d/ | Fourth photo |
| video | FileField | livestock/videos/%Y/%m/%d/ | MP4/MOV/AVI/WEBM |

##### F. Business (5 fields) ⭐ NEW
| Field | Type | Default | Description |
|-------|------|---------|-------------|
| is_featured | Boolean | False | Show on homepage |
| is_active | Boolean | True | Publicly visible |
| views_count | Integer | 0 | Page view counter |
| rating | Float | 0.0 | 0.0 to 5.0 stars |
| type | CharField | SiNGL_PR | Product type enum |

##### G. Social Media (3 fields) ⭐ NEW
| Field | Type | Max Length | Description |
|-------|------|------------|-------------|
| facebook_post_id | CharField | 100 | Facebook post reference |
| instagram_post_id | CharField | 100 | Instagram post reference |
| tiktok_post_id | CharField | 100 | TikTok video reference |

##### H. Relationships (2 fields) ✅ FIXED
| Field | Type | Related Model | Description |
|-------|------|---------------|-------------|
| owner | ForeignKey | User | Product owner (seller) |
| market_k | ForeignKey | Market | Associated market |

**Indexes:**
1. `idx_product_created` ON `created_at DESC` - Fast "newest first" queries
2. `idx_product_location` ON `governorate, breed` - Location-based filtering
3. `idx_product_visibility` ON `is_active, is_featured` - Homepage queries

**Total Fields:** 31 (was 10) → **+21 new fields**

---

### 2. **MARKET**

**Table Name:** `gr_market`  
**Purpose:** Group products by market category

| Field | Type | Description |
|-------|------|-------------|
| id | AutoIncrement | Primary key |
| uuid | UUID | Public identifier |
| name | CharField(100) | Market name |
| mark_number | CharField(50) | Market category enum |
| owner | ForeignKey | User who created market ✅ FIXED |
| created_at | DateTime | Creation timestamp ⭐ NEW |
| updated_at | DateTime | Last update ⭐ NEW |

**Relationships:**
- `owner` → User (Many-to-One) ✅ Restored
- `products` ← Product.market_k (One-to-Many)

**Total Fields:** 7 (was 4) → **+3 new fields**

---

### 3. **ORDER**

**Table Name:** `gr_order`  
**Purpose:** Track customer orders

| Field | Type | Choices | Description |
|-------|------|---------|-------------|
| id | AutoIncrement | PK | Order ID |
| order_date | DateTime | Auto | Order creation |
| status | CharField | idle, pending, delivered, canceled | Order status |

**Relationships:**
- `order_items` ← OrderItem.item_K (One-to-Many)
- `payment` ← Payment.order (One-to-One)

---

### 4. **ORDER_ITEM**

**Table Name:** `gr_orderitem`  
**Purpose:** Individual products in an order

| Field | Type | Description |
|-------|------|-------------|
| id | AutoIncrement | Primary key |
| uuid | UUID | Public identifier |
| product | ForeignKey | Product being purchased |
| quantity | Integer | Quantity ordered |
| price | Decimal(10,2) | Price at time of order |
| owner | ForeignKey | User who placed order |
| item_K | ForeignKey | Associated order |

**Relationships:**
- `product` → Product (Many-to-One)
- `owner` → User (Many-to-One)
- `item_K` → Order (Many-to-One)

---

### 5. **PAYMENT**

**Table Name:** `gr_payment`  
**Purpose:** Payment records for orders

| Field | Type | Choices | Description |
|-------|------|---------|-------------|
| id | AutoIncrement | PK | Payment ID |
| uuid | UUID | Unique | Public identifier |
| payment_method | CharField | cash, Rip | Payment type |
| singel_amount | Decimal(10,2) | | Single item amount |
| group_amount | Decimal(10,2) | | Group/bulk amount |
| owner | ForeignKey | User | Payment maker |
| order | ForeignKey | Order | Associated order |
| stock_k | ForeignKey | Product | Product purchased |

**Note:** Cash on Delivery (COD) is primary payment method for Tunisia

**Relationships:**
- `owner` → User (Many-to-One)
- `order` → Order (Many-to-One)
- `stock_k` → Product (Many-to-One)

---

## 🔗 Relationship Summary

```
User (1) ──┬─> (Many) Product [owner]
           ├─> (Many) Market [owner]
           ├─> (Many) Order_Item [owner]
           └─> (Many) Payment [owner]

Market (1) ──> (Many) Product [market_k]

Product (1) ──┬─> (Many) Order_Item [product]
              └─> (Many) Payment [stock_k]

Order (1) ──┬─> (Many) Order_Item [item_K]
            └─> (1) Payment [order]
```

---

## 📈 Database Size Estimates

For a growing livestock platform:

| Scale | Products | Users | Orders/Month | Storage |
|-------|----------|-------|--------------|---------|
| Small | 100-500 | 50 | 50-100 | ~1 GB |
| Medium | 500-2,000 | 200 | 200-500 | ~5 GB |
| Large | 2,000-10,000 | 1,000 | 1,000-2,000 | ~20 GB |

**Storage Breakdown:**
- Product records: ~10 KB each
- Images (4 per product): ~2-3 MB each = 8-12 MB per product
- Videos: ~10-50 MB each
- Database indexes: ~10-15% overhead

**Example:** 1,000 products with photos = ~10 GB

---

## 🎯 Key Database Features

### 1. **Livestock-Specific Schema**
- ✅ Breed tracking (تبار, غربي, سردي, سويسري)
- ✅ Gender classification (ذكر, أنثى, مختلط)
- ✅ Physical attributes (age, weight)
- ✅ Quantity tracking for bulk sales

### 2. **Tunisia-First Design**
- ✅ All 24 Tunisian governorates
- ✅ City/delegation field
- ✅ 3 decimal places for TND currency
- ✅ Cash on Delivery payment primary

### 3. **Media Management**
- ✅ 4 images per product
- ✅ 1 video per product
- ✅ Organized by date (year/month/day)

### 4. **Social Media Integration**
- ✅ Facebook post tracking
- ✅ Instagram post tracking
- ✅ TikTok video tracking

### 5. **Performance Optimization**
- ✅ Index on created_at (newest listings first)
- ✅ Composite index on governorate+breed (location filtering)
- ✅ Index on is_active+is_featured (homepage queries)

---

## 🔍 Sample Queries

### Get Active Livestock in Béja
```sql
SELECT product_name, breed, gender, age_months, weight_kg, price, city
FROM gr_product
WHERE governorate = 'باجة' AND is_active = TRUE
ORDER BY created_at DESC;
```

### Get Featured Sardi Sheep
```sql
SELECT product_name, weight_kg, price, wholesale_price, city
FROM gr_product
WHERE breed = 'سردي' AND is_featured = TRUE AND is_active = TRUE
ORDER BY views_count DESC;
```

### Get Wholesale Deals
```sql
SELECT product_name, breed, quantity_available, price, wholesale_price,
       (price - wholesale_price) as savings_per_unit
FROM gr_product
WHERE quantity_available >= min_wholesale_quantity
  AND wholesale_price < price
  AND is_active = TRUE
ORDER BY savings_per_unit DESC;
```

### Get User's Livestock Listings
```sql
SELECT p.product_name, p.breed, p.gender, p.quantity_available, p.governorate,
       u.username as seller
FROM gr_product p
JOIN users_user u ON p.owner_id = u.id
WHERE u.username = 'your_username'
ORDER BY p.created_at DESC;
```

---

## ✅ Migration Checklist

After running migrations, verify these tables exist:

```sql
-- List all tables
\dt

Expected tables:
  ✓ gr_product      (31 fields)
  ✓ gr_market       (7 fields)
  ✓ gr_order        (3 fields)
  ✓ gr_orderitem    (7 fields)
  ✓ gr_payment      (8 fields)
  ✓ users_user      (Django auth)
  ✓ auth_*          (Django auth tables)
  ✓ django_*        (Django system tables)
```

---

## 🎉 Database Schema Complete!

Your database now supports:
- ✅ Complete livestock management
- ✅ Tunisian location system
- ✅ Multi-image uploads
- ✅ Wholesale pricing
- ✅ Social media integration
- ✅ Performance optimization
- ✅ E-commerce functionality

**Ready for production use! 🚀**
