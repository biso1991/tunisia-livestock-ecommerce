# 🚀 Quick Start Guide - Agriculture Livestock E-commerce

**Project:** Tunisia Sheep Trading Platform (مربي الماشية بمجاز الباب)  
**Location:** Majaz El Bab, Béja, Tunisia  
**Status:** Backend Step 1 Complete ✅ | Ready for Migrations

---

## ⚡ 3-Minute Quick Start

### Step 1: Run Migrations (Choose One Method)

#### Option A: Automated Script (Easiest) ⭐
```bash
cd /home/bilel/Desktop/agriculture_project/backend
./RUN_MIGRATIONS.sh
```

That's it! The script will:
- Pre-pull Docker images
- Start PostgreSQL and Redis
- Run migrations automatically
- Start Django server
- Give you access URLs

---

#### Option B: Manual Docker Commands
```bash
cd /home/bilel/Desktop/agriculture_project/backend

# Pre-pull images to avoid timeout
docker pull postgres:11.6
docker pull redis:6.2-alpine

# Start services
docker-compose up -d postgres redis

# Wait for PostgreSQL (15 seconds)
sleep 15

# Install new dependencies and run migrations
docker-compose run --rm web bash -c "pip install Pillow python-dotenv && ./manage.py makemigrations"
docker-compose run --rm web ./manage.py migrate

# Start Django
docker-compose up -d web

# Check logs
docker-compose logs -f web
```

---

### Step 2: Access Your Platform

Once migrations complete:

**🎛️ Django Admin Panel:**
```
URL: http://localhost:8000/admin
Username: root
Password: 1991
```

**🔌 API Endpoints:**
```
Base URL: http://localhost:8000/api/
Products: http://localhost:8000/api/products/
Markets: http://localhost:8000/api/markets/
```

**💾 PgAdmin (Database Manager):**
```
URL: http://localhost:5050
Email: dbuser@gmail.com
Password: 1991
```

---

## 📝 Create Your First Livestock Product

### Via Django Admin (Easiest)

1. Go to http://localhost:8000/admin
2. Login with root/1991
3. Click "Products" → "Add Product"
4. Fill in:

**Basic Information:**
- Product Name: `كبش سردي عمر 18 شهر`
- Description: `كبش سردي ممتاز، وزن 65 كيلو، جاهز للبيع`
- Owner: Select your user
- Market: (optional)

**Livestock Details:**
- Breed: `سردي` (Sardi)
- Gender: `ذكر` (Male)
- Age: `18` months
- Weight: `65.5` kg
- Quantity: `3` animals

**Pricing:**
- Price: `1500.000` TND
- Wholesale Price: `1350.000` TND
- Min Wholesale Quantity: `5`

**Location:**
- Governorate: `باجة` (Béja)
- City: `مجاز الباب` (Majaz El Bab)

**Media:**
- Upload 1-4 photos of your sheep
- Upload video (optional)

**Status:**
- Is Active: ✅ Checked
- Is Featured: ✅ Checked (for homepage)

5. Click "Save"

---

### Via API (For Developers)

```bash
# Get auth token first
curl -X POST http://localhost:8000/api/auth/login/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "root",
    "password": "1991"
  }'

# Create product (replace YOUR_TOKEN with token from above)
curl -X POST http://localhost:8000/api/products/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "product_name": "كبش سردي عمر 18 شهر",
    "description": "كبش سردي ممتاز، وزن 65 كيلو",
    "breed": "سردي",
    "gender": "ذكر",
    "age_months": 18,
    "weight_kg": 65.5,
    "quantity_available": 3,
    "price": "1500.000",
    "wholesale_price": "1350.000",
    "min_wholesale_quantity": 5,
    "governorate": "باجة",
    "city": "مجاز الباب",
    "is_active": true,
    "is_featured": true
  }'
```

---

## 🔍 Verify Everything Works

### 1. Check Database Tables
```bash
docker exec -it postgres psql -U dbuser -d db
```

Then in psql:
```sql
-- List all tables
\dt

-- Check Product table structure
\d gr_product

-- Count products
SELECT COUNT(*) FROM gr_product;

-- View products with livestock info
SELECT product_name, breed, gender, age_months, weight_kg, governorate, city 
FROM gr_product;
```

Type `\q` to exit psql.

---

### 2. Check Django Admin Features

In http://localhost:8000/admin, verify:
- ✅ Can see "Products" model
- ✅ Can add new product
- ✅ Can see livestock fields (breed, gender, age, weight)
- ✅ Can select governorate from dropdown (all 24 governorates)
- ✅ Can upload images
- ✅ Can set wholesale pricing
- ✅ Foreign keys work (owner, market)

---

### 3. Test API Endpoints

```bash
# Get all products
curl http://localhost:8000/api/products/

# Get single product
curl http://localhost:8000/api/products/1/

# Filter by governorate
curl http://localhost:8000/api/products/?governorate=باجة

# Filter by breed
curl http://localhost:8000/api/products/?breed=سردي

# Search
curl http://localhost:8000/api/products/?search=كبش
```

---

## 🛑 Common Issues & Solutions

### Issue 1: Docker Timeout on Image Pull
**Error:** `docker-compose up` hangs forever  
**Solution:**
```bash
# Pre-pull images separately
docker pull postgres:11.6
docker pull redis:6.2-alpine
docker pull dpage/pgadmin4

# Then start
docker-compose up -d
```

---

### Issue 2: "relation does not exist" Error
**Error:** `relation "gr_product" does not exist`  
**Cause:** Migrations not run  
**Solution:**
```bash
docker-compose run --rm web ./manage.py migrate
```

---

### Issue 3: Missing Pillow Module
**Error:** `No module named 'PIL'`  
**Cause:** Pillow not installed for image uploads  
**Solution:**
```bash
docker-compose run --rm web pip install Pillow python-dotenv
docker-compose restart web
```

---

### Issue 4: CORS Errors from Frontend
**Error:** `Access to XMLHttpRequest blocked by CORS policy`  
**Status:** ✅ Already fixed in settings  
**Verify:** Check `api/config/common.py` has:
```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:4200",
    "http://127.0.0.1:4200",
]
```

---

### Issue 5: Port Already in Use
**Error:** `port is already allocated`  
**Solution:**
```bash
# Stop all containers
docker-compose down

# Check what's using port 8000
sudo lsof -i :8000

# Kill the process or change port in docker-compose.yml
```

---

## 📊 What Was Fixed in Step 1?

| Component | Issue | Fix | Status |
|-----------|-------|-----|--------|
| Product Model | Foreign keys commented out | Uncommented & restored | ✅ Fixed |
| Product Model | Wrong field labels ("library") | Changed to livestock terms | ✅ Fixed |
| Product Model | No livestock fields | Added 11 livestock fields | ✅ Added |
| Product Model | No location system | Added 24 governorates | ✅ Added |
| Product Model | No media support | Added 4 images + video | ✅ Added |
| Product Model | No wholesale pricing | Added bulk pricing | ✅ Added |
| Settings | Empty CORS config | Added Angular URLs | ✅ Fixed |
| Settings | No .env file | Created with all secrets | ✅ Created |
| Dependencies | No Pillow for images | Added to requirements | ✅ Added |
| Admin | Basic interface | Enhanced with fieldsets | ✅ Improved |

---

## 🎯 Next Steps

### Immediate (5 minutes)
- ✅ Run migrations (see above)
- ✅ Create superuser (if needed)
- ✅ Add 1-3 test products in admin

### Backend Completion (2-3 hours)
- 🔲 Create API serializers for Product model
- 🔲 Create API viewsets with filters
- 🔲 Add search functionality
- 🔲 Add authentication/permissions
- 🔲 Write API tests

### Frontend Development (4-6 hours)
- 🔲 Initialize Angular project with RTL
- 🔲 Setup i18n (Arabic/French/English)
- 🔲 Create components (home, catalog, detail, cart)
- 🔲 Connect to Django API
- 🔲 Mobile-responsive design
- 🔲 Add filters (breed, gender, governorate)

### Business Features (2-3 hours)
- 🔲 Social media sharing
- 🔲 WhatsApp integration for COD
- 🔲 Seller dashboard
- 🔲 Analytics (views, clicks)
- 🔲 Featured products on homepage

---

## 🆘 Need Help?

### Check Logs
```bash
# Django logs
docker-compose logs -f web

# PostgreSQL logs
docker-compose logs -f postgres

# All logs
docker-compose logs -f
```

### Restart Services
```bash
# Restart everything
docker-compose restart

# Restart just Django
docker-compose restart web

# Full rebuild
docker-compose down
docker-compose build --no-cache
docker-compose up -d
```

### Database Access
```bash
# Enter PostgreSQL
docker exec -it postgres psql -U dbuser -d db

# Backup database
docker exec postgres pg_dump -U dbuser db > backup.sql

# Restore database
cat backup.sql | docker exec -i postgres psql -U dbuser -d db
```

---

## 📞 What to Tell Me

If something doesn't work, send me:

1. **What command you ran:**
   ```
   docker-compose up -d
   ```

2. **The error message:**
   ```
   Error: ...
   ```

3. **What logs show:**
   ```bash
   docker-compose logs web | tail -50
   ```

Then I can help immediately! 🚀

---

## ✅ Success Checklist

After migrations, verify:

- [ ] Django admin loads at http://localhost:8000/admin
- [ ] Can login with root/1991
- [ ] Can see "Products" with livestock fields
- [ ] Can add new product
- [ ] Can upload images
- [ ] Can see governorate dropdown (24 options in Arabic)
- [ ] Can set wholesale pricing
- [ ] Foreign keys work (owner, market)
- [ ] API returns data: `curl http://localhost:8000/api/products/`

**All checked? You're ready for Step 2: Angular Frontend! 🎉**

---

## 📚 Documentation Files

Created in this step:

1. **STEP1_BACKEND_FIXES_COMPLETE.md** - Full technical documentation
2. **BACKEND_TRANSFORMATION.md** - Before/after comparison
3. **QUICKSTART.md** - This file (quick reference)
4. **RUN_MIGRATIONS.sh** - Automated migration script
5. **.env** - Environment configuration

All located in: `/home/bilel/Desktop/agriculture_project/`

---

**Ready to Start? Run:** `./RUN_MIGRATIONS.sh` 🚀
