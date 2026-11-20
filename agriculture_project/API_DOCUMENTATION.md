# 🔌 API Documentation - Agriculture Livestock E-commerce

**Project:** Tunisia Sheep Trading Platform (مربي الماشية بمجاز الباب)  
**Base URL:** `http://localhost:8000`  
**API Version:** v1  
**Authentication:** Token-based

---

## 📋 Table of Contents

1. [Authentication](#authentication)
2. [Product/Livestock API](#product-livestock-api)
3. [Market API](#market-api)
4. [Order & Payment API](#order--payment-api)
5. [Filter & Search Examples](#filter--search-examples)
6. [Error Handling](#error-handling)
7. [Testing with cURL](#testing-with-curl)

---

## 🔐 Authentication

### Get Auth Token
```http
POST /api-token-auth/
Content-Type: application/json

{
  "username": "root",
  "password": "1991"
}
```

**Response:**
```json
{
  "token": "9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b"
}
```

### Using Token
Include in all authenticated requests:
```http
Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b
```

---

## 🐑 Product/Livestock API

Base endpoint: `/api/gr/products/`

### List Products
**GET** `/api/gr/products/`

**Public:** Yes (no auth required)  
**Filters:** breed, gender, governorate, city, price, age, weight  
**Search:** product_name, description, city  
**Ordering:** created_at, price, views_count, rating

**Example Request:**
```bash
curl http://localhost:8000/api/gr/products/
```

**Response:**
```json
{
  "count": 10,
  "next": "http://localhost:8000/api/gr/products/?page=2",
  "previous": null,
  "results": [
    {
      "id": 1,
      "uuid": "a1b2c3d4-...",
      "product_name": "كبش سردي عمر 18 شهر",
      "breed": "سردي",
      "gender": "ذكر",
      "age_months": 18,
      "weight_kg": "65.50",
      "quantity_available": 3,
      "price": "1500.000",
      "wholesale_price": "1350.000",
      "has_wholesale": true,
      "governorate": "باجة",
      "city": "مجاز الباب",
      "is_featured": true,
      "is_active": true,
      "views_count": 45,
      "rating": 4.5,
      "thumbnail": "http://localhost:8000/media/livestock/images/2024/11/20/sheep1.jpg",
      "owner_username": "farmer_majaz",
      "market_name": "Béja Livestock Market",
      "created_at": "2024-11-20T10:30:00Z"
    }
  ]
}
```

---

### Get Single Product
**GET** `/api/gr/products/{id}/`

**Public:** Yes  
**Note:** Automatically increments view count

**Example:**
```bash
curl http://localhost:8000/api/gr/products/1/
```

**Response:**
```json
{
  "id": 1,
  "uuid": "a1b2c3d4-...",
  "product_name": "كبش سردي عمر 18 شهر",
  "description": "كبش سردي ممتاز، وزن 65 كيلو، جاهز للبيع",
  "breed": "سردي",
  "gender": "ذكر",
  "age_months": 18,
  "weight_kg": "65.50",
  "quantity_available": 3,
  "price": "1500.000",
  "wholesale_price": "1350.000",
  "min_wholesale_quantity": 5,
  "wholesale_savings": 150.0,
  "governorate": "باجة",
  "city": "مجاز الباب",
  "images": [
    "http://localhost:8000/media/livestock/images/2024/11/20/sheep1_main.jpg",
    "http://localhost:8000/media/livestock/images/2024/11/20/sheep1_2.jpg",
    "http://localhost:8000/media/livestock/images/2024/11/20/sheep1_3.jpg",
    "http://localhost:8000/media/livestock/images/2024/11/20/sheep1_4.jpg"
  ],
  "video_url": "http://localhost:8000/media/livestock/videos/2024/11/20/sheep1.mp4",
  "is_featured": true,
  "is_active": true,
  "views_count": 46,
  "rating": 4.5,
  "stock": 3,
  "type": "Single Product",
  "Buy_sell": "seller",
  "owner": 1,
  "owner_username": "farmer_majaz",
  "owner_email": "farmer@example.com",
  "market_k": 1,
  "market_name": "Béja Livestock Market",
  "facebook_post_id": "123456789",
  "instagram_post_id": "ABC123",
  "tiktok_post_id": "XYZ789",
  "created_at": "2024-11-20T10:30:00Z",
  "updated_at": "2024-11-20T15:45:00Z"
}
```

---

### Create Product
**POST** `/api/gr/products/`

**Auth Required:** Yes  
**Permissions:** Authenticated users

**Request:**
```bash
curl -X POST http://localhost:8000/api/gr/products/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "product_name": "كبش غربي عمر 24 شهر",
    "description": "كبش غربي ممتاز للذبح أو التربية",
    "breed": "غربي",
    "gender": "ذكر",
    "age_months": 24,
    "weight_kg": 75.5,
    "quantity_available": 2,
    "price": 1800.000,
    "wholesale_price": 1650.000,
    "min_wholesale_quantity": 4,
    "governorate": "باجة",
    "city": "مجاز الباب",
    "is_active": true,
    "is_featured": false
  }'
```

**With Images (multipart/form-data):**
```bash
curl -X POST http://localhost:8000/api/gr/products/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -F "product_name=كبش غربي" \
  -F "breed=غربي" \
  -F "gender=ذكر" \
  -F "age_months=24" \
  -F "weight_kg=75.5" \
  -F "quantity_available=2" \
  -F "price=1800.000" \
  -F "governorate=باجة" \
  -F "city=مجاز الباب" \
  -F "image_main=@/path/to/sheep_main.jpg" \
  -F "image_2=@/path/to/sheep_2.jpg" \
  -F "video=@/path/to/sheep_video.mp4"
```

**Response:** 201 Created
```json
{
  "id": 2,
  "uuid": "b2c3d4e5-...",
  "product_name": "كبش غربي عمر 24 شهر",
  ...
}
```

---

### Update Product
**PUT/PATCH** `/api/gr/products/{id}/`

**Auth Required:** Yes  
**Permissions:** Owner or Admin only

**Example:**
```bash
curl -X PATCH http://localhost:8000/api/gr/products/1/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "price": 1450.000,
    "quantity_available": 2
  }'
```

---

### Delete Product
**DELETE** `/api/gr/products/{id}/`

**Auth Required:** Yes  
**Permissions:** Owner or Admin only

**Example:**
```bash
curl -X DELETE http://localhost:8000/api/gr/products/1/ \
  -H "Authorization: Token YOUR_TOKEN"
```

**Response:** 204 No Content

---

### Custom Product Endpoints

#### Featured Products (Homepage)
**GET** `/api/gr/products/featured/`

**Public:** Yes

```bash
curl http://localhost:8000/api/gr/products/featured/
```

Returns top 10 featured products by views.

---

#### My Products
**GET** `/api/gr/products/my_products/`

**Auth Required:** Yes

```bash
curl http://localhost:8000/api/gr/products/my_products/ \
  -H "Authorization: Token YOUR_TOKEN"
```

Returns all products owned by authenticated user.

---

#### Products by User
**GET** `/api/gr/products/by-user/{username}/`

**Public:** Yes

```bash
curl http://localhost:8000/api/gr/products/by-user/farmer_majaz/
```

---

#### Products by Governorate
**GET** `/api/gr/products/governorate/{governorate}/`

**Public:** Yes

```bash
curl http://localhost:8000/api/gr/products/governorate/باجة/
```

---

#### Wholesale Products
**GET** `/api/gr/products/wholesale/`

**Public:** Yes

```bash
curl http://localhost:8000/api/gr/products/wholesale/
```

Returns products with active wholesale pricing.

---

#### Product Statistics
**GET** `/api/gr/products/statistics/`

**Auth Required:** Yes

```bash
curl http://localhost:8000/api/gr/products/statistics/ \
  -H "Authorization: Token YOUR_TOKEN"
```

**Response:**
```json
{
  "total_products": 15,
  "active_products": 12,
  "featured_products": 3,
  "total_views": 456,
  "total_quantity": 45,
  "avg_price": 1650.50,
  "by_breed": {
    "سردي": 5,
    "غربي": 7,
    "تبار": 3
  },
  "by_governorate": {
    "باجة": 10,
    "تونس": 5
  }
}
```

---

## 🏪 Market API

Base endpoint: `/api/gr/markets/`

### List Markets
**GET** `/api/gr/markets/`

**Public:** Yes

```bash
curl http://localhost:8000/api/gr/markets/
```

---

### Create Market
**POST** `/api/gr/markets/`

**Auth Required:** Yes

```bash
curl -X POST http://localhost:8000/api/gr/markets/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Majaz El Bab Livestock Market",
    "mark_number": "product_one"
  }'
```

---

### Market Products
**GET** `/api/gr/markets/{id}/products/`

**Public:** Yes

```bash
curl http://localhost:8000/api/gr/markets/1/products/
```

Returns all active products in the market.

---

## 💰 Order & Payment API

### List Orders
**GET** `/api/gr/orders/`

**Auth Required:** Yes

```bash
curl http://localhost:8000/api/gr/orders/ \
  -H "Authorization: Token YOUR_TOKEN"
```

---

### Create Order
**POST** `/api/gr/orders/`

**Auth Required:** Yes

```bash
curl -X POST http://localhost:8000/api/gr/orders/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "pending"
  }'
```

---

### Add Order Item
**POST** `/api/gr/order-items/`

**Auth Required:** Yes

```bash
curl -X POST http://localhost:8000/api/gr/order-items/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "product": 1,
    "quantity": 2,
    "price": 1500.000,
    "item_K": 1
  }'
```

---

### Create Payment
**POST** `/api/gr/payments/`

**Auth Required:** Yes

```bash
curl -X POST http://localhost:8000/api/gr/payments/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "payment_method": "cash",
    "singel_amount": 3000.000,
    "order": 1,
    "stock_k": 1
  }'
```

---

## 🔍 Filter & Search Examples

### Filter by Breed
```bash
curl "http://localhost:8000/api/gr/products/?breed=سردي"
```

### Filter by Gender
```bash
curl "http://localhost:8000/api/gr/products/?gender=ذكر"
```

### Filter by Governorate
```bash
curl "http://localhost:8000/api/gr/products/?governorate=باجة"
```

### Price Range
```bash
curl "http://localhost:8000/api/gr/products/?price__gte=1000&price__lte=2000"
```

### Age Range
```bash
curl "http://localhost:8000/api/gr/products/?age_months__gte=12&age_months__lte=24"
```

### Weight Range
```bash
curl "http://localhost:8000/api/gr/products/?weight_kg__gte=50&weight_kg__lte=80"
```

### Featured Products Only
```bash
curl "http://localhost:8000/api/gr/products/?is_featured=true"
```

### Search by Name/Description/City
```bash
curl "http://localhost:8000/api/gr/products/?search=كبش"
```

### Combined Filters
```bash
curl "http://localhost:8000/api/gr/products/?breed=سردي&governorate=باجة&price__lte=1500&ordering=-views_count"
```

---

## 📊 Ordering

### Order by Price (Ascending)
```bash
curl "http://localhost:8000/api/gr/products/?ordering=price"
```

### Order by Price (Descending)
```bash
curl "http://localhost:8000/api/gr/products/?ordering=-price"
```

### Order by Views (Most Viewed First)
```bash
curl "http://localhost:8000/api/gr/products/?ordering=-views_count"
```

### Order by Date (Newest First)
```bash
curl "http://localhost:8000/api/gr/products/?ordering=-created_at"
```

### Order by Rating
```bash
curl "http://localhost:8000/api/gr/products/?ordering=-rating"
```

---

## ❌ Error Handling

### 400 Bad Request
```json
{
  "field_name": ["Error message"],
  "another_field": ["Error message"]
}
```

### 401 Unauthorized
```json
{
  "detail": "Authentication credentials were not provided."
}
```

### 403 Forbidden
```json
{
  "detail": "You do not have permission to perform this action."
}
```

### 404 Not Found
```json
{
  "detail": "Not found."
}
```

### 500 Internal Server Error
```json
{
  "detail": "Internal server error."
}
```

---

## 🧪 Testing with cURL

### Complete Example: Create and Retrieve Product

1. **Get Token:**
```bash
TOKEN=$(curl -s -X POST http://localhost:8000/api-token-auth/ \
  -H "Content-Type: application/json" \
  -d '{"username":"root","password":"1991"}' \
  | jq -r '.token')

echo "Token: $TOKEN"
```

2. **Create Product:**
```bash
curl -X POST http://localhost:8000/api/gr/products/ \
  -H "Authorization: Token $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "product_name": "كبش تبار للبيع",
    "description": "كبش تبار أصيل، عمر 20 شهر",
    "breed": "تبار",
    "gender": "ذكر",
    "age_months": 20,
    "weight_kg": 70.0,
    "quantity_available": 1,
    "price": 1600.000,
    "governorate": "باجة",
    "city": "مجاز الباب",
    "is_active": true
  }' | jq
```

3. **Get All Products:**
```bash
curl http://localhost:8000/api/gr/products/ | jq
```

4. **Filter by Béja Governorate:**
```bash
curl "http://localhost:8000/api/gr/products/?governorate=باجة" | jq
```

5. **Get My Products:**
```bash
curl http://localhost:8000/api/gr/products/my_products/ \
  -H "Authorization: Token $TOKEN" | jq
```

6. **Get Statistics:**
```bash
curl http://localhost:8000/api/gr/products/statistics/ \
  -H "Authorization: Token $TOKEN" | jq
```

---

## 📝 Field Reference

### Product Model Fields

| Field | Type | Required | Choices | Description |
|-------|------|----------|---------|-------------|
| product_name | string(200) | Yes | - | Product name in Arabic/French |
| description | text | No | - | Detailed description |
| breed | string | No | تبار, غربي, سردي, سويسري, مختلط, أخرى | Sheep breed |
| gender | string | No | ذكر, أنثى, مختلط | Male/Female/Mixed |
| age_months | integer | No | - | Age in months |
| weight_kg | decimal(6,2) | No | - | Weight in kilograms |
| quantity_available | integer | Yes | - | Number available |
| price | decimal(10,3) | Yes | - | Price in TND |
| wholesale_price | decimal(10,3) | No | - | Bulk price in TND |
| min_wholesale_quantity | integer | No | - | Min quantity for wholesale |
| governorate | string | No | 24 options | Tunisian governorate |
| city | string(100) | No | - | City/delegation name |
| image_main | file | No | jpg,png | Main image |
| image_2,3,4 | file | No | jpg,png | Additional images |
| video | file | No | mp4,mov,avi,webm | Product video |
| is_featured | boolean | No | - | Featured on homepage |
| is_active | boolean | No | - | Publicly visible |

---

## 🎯 Quick Reference

**Base URLs:**
- Development: `http://localhost:8000`
- Admin: `http://localhost:8000/admin/`
- API Root: `http://localhost:8000/api/gr/`

**Authentication:**
- Get token: `POST /api-token-auth/`
- Header: `Authorization: Token {token}`

**Main Endpoints:**
- Products: `/api/gr/products/`
- Markets: `/api/gr/markets/`
- Orders: `/api/gr/orders/`
- Payments: `/api/gr/payments/`

**Common Filters:**
- `?breed=سردي`
- `?governorate=باجة`
- `?price__gte=1000&price__lte=2000`
- `?search=كبش`
- `?ordering=-views_count`

---

## 🚀 Next Steps

1. Run migrations to create database schema
2. Test endpoints with cURL or Postman
3. Create frontend Angular application
4. Connect Angular to these API endpoints
5. Add social media sharing
6. Implement WhatsApp integration for COD

---

**API Documentation Complete!** 🎉

For Angular integration, use `HttpClient` with these endpoints.
