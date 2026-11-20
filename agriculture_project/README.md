# 🐑 Tunisia Livestock E-commerce Platform

**مربي الماشية بمجاز الباب** - Modern livestock trading platform for Tunisia

[![Django](https://img.shields.io/badge/Django-4.0.1-green.svg)](https://www.djangoproject.com/)
[![DRF](https://img.shields.io/badge/DRF-3.13-red.svg)](https://www.django-rest-framework.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-11.6-blue.svg)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED.svg)](https://www.docker.com/)
[![Angular](https://img.shields.io/badge/Angular-Coming_Soon-red.svg)](https://angular.io/)

---

## 📋 Overview

A comprehensive e-commerce platform specifically designed for livestock trading in Tunisia, with focus on sheep (Tabar, Gharbi, Sardi, Swiss breeds). Built for farmers in **Majaz El Bab, Béja** and across Tunisia.

### Key Features

- 🐑 **Livestock-Specific:** Breed classification, gender, age, weight tracking
- 🇹🇳 **Tunisia-First:** All 24 governorates, TND currency, Arabic RTL support
- 📸 **Rich Media:** 4 images + 1 video per product
- 💰 **Wholesale Pricing:** Bulk discounts for large orders
- 🔍 **Advanced Filters:** Search by breed, location, price, age, weight
- 📱 **Social Media:** Facebook, Instagram, TikTok integration
- 💳 **Cash on Delivery:** Primary payment method for Tunisia
- 📊 **Analytics:** Seller dashboard with statistics

---

## 🏗️ Architecture

### Backend (Django REST API)
```
backend/
├── api/
│   ├── gr/              # Livestock/Products API
│   │   ├── models.py    # Database models (31 fields)
│   │   ├── serializers.py  # API serializers (11 types)
│   │   ├── views.py     # ViewSets with filters
│   │   ├── admin.py     # Enhanced admin interface
│   │   └── urls.py      # API routing
│   ├── users/           # Authentication
│   ├── blogs/           # Blog functionality
│   └── config/          # Django settings
├── docker-compose.yml   # Docker orchestration
├── requirements.txt     # Python dependencies
└── .env                 # Environment variables
```

### Frontend (Angular - Coming Soon)
```
frontend/
├── src/
│   ├── app/
│   │   ├── core/        # Services, guards, interceptors
│   │   ├── shared/      # Shared components
│   │   ├── features/    # Feature modules
│   │   └── i18n/        # Arabic/French/English
│   └── assets/
└── angular.json
```

---

## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- Git
- (Optional) Python 3.8+, Node.js 16+

### 1. Clone Repository
```bash
git clone https://github.com/biso1991/tunisia-livestock-platform.git
cd tunisia-livestock-platform
```

### 2. Run Backend
```bash
cd backend
./RUN_MIGRATIONS.sh    # Automated setup
# OR
docker-compose up -d   # Manual start
```

### 3. Access Application
- **Django Admin:** http://localhost:8000/admin/ (root/1991)
- **API Root:** http://localhost:8000/api/gr/
- **API Docs:** See [API_DOCUMENTATION.md](API_DOCUMENTATION.md)
- **PgAdmin:** http://localhost:5050 (dbuser@gmail.com/1991)

---

## 📊 Database Schema

### Product Model (Livestock)
31 fields including:
- **Livestock:** breed, gender, age_months, weight_kg, quantity_available
- **Location:** governorate (24 options), city
- **Media:** 4 images + video
- **Pricing:** price, wholesale_price, min_wholesale_quantity
- **Business:** is_featured, is_active, views_count
- **Social:** facebook_post_id, instagram_post_id, tiktok_post_id

### Supported Breeds
- تبار (Tabar)
- غربي (Gharbi)
- سردي (Sardi)
- سويسري (Swiss)
- مختلط (Mixed)
- أخرى (Other)

### Tunisian Governorates
All 24 governorates supported:
باجة، تونس، أريانة، بن عروس، منوبة، نابل، زغوان، بنزرت، جندوبة، الكاف، سليانة، القيروان، القصرين، سيدي بوزيد، سوسة، المنستير، المهدية، صفاقس، قابس، مدنين، تطاوين، قفصة، توزر، قبلي

---

## 🔌 API Endpoints

### Products (Livestock)
```http
GET    /api/gr/products/                    # List all products
POST   /api/gr/products/                    # Create product (auth)
GET    /api/gr/products/{id}/               # Get single product
PUT    /api/gr/products/{id}/               # Update (owner/admin)
DELETE /api/gr/products/{id}/               # Delete (owner/admin)
GET    /api/gr/products/featured/           # Featured products
GET    /api/gr/products/my_products/        # User's products (auth)
GET    /api/gr/products/by-user/{username}/ # By username
GET    /api/gr/products/governorate/{gov}/  # By governorate
GET    /api/gr/products/wholesale/          # Wholesale deals
GET    /api/gr/products/statistics/         # User stats (auth)
```

### Filters
```
?breed=سردي                      # Filter by breed
?gender=ذكر                       # Filter by gender  
?governorate=باجة                 # Filter by governorate
?price__gte=1000&price__lte=2000 # Price range
?age_months__gte=12              # Minimum age
?weight_kg__gte=50               # Minimum weight
?search=كبش                       # Search
?ordering=-views_count           # Sort by views
```

### Example Request
```bash
# Get all Sardi sheep in Béja under 1500 TND
curl "http://localhost:8000/api/gr/products/?breed=سردي&governorate=باجة&price__lte=1500"
```

---

## 🛠️ Technology Stack

### Backend
- **Django 4.0.1** - Web framework
- **Django REST Framework 3.13** - API
- **PostgreSQL 11.6** - Database
- **Redis 6.2** - Caching
- **Celery** - Background tasks
- **Docker** - Containerization
- **Pillow** - Image processing

### Frontend (Planned)
- **Angular 15+** - Frontend framework
- **Angular Material** - UI components
- **@ngx-translate** - i18n (Arabic/French/English)
- **RxJS** - Reactive programming

### DevOps
- **Docker Compose** - Local development
- **GitHub Actions** - CI/CD (planned)
- **Heroku/AWS** - Deployment (planned)

---

## 📈 Development Status

**Backend: 95% Complete** ✅
- [x] Models with 31 fields
- [x] 11 API serializers
- [x] 5 ViewSets with full CRUD
- [x] 25+ API endpoints
- [x] 10+ filter types
- [x] Authentication & permissions
- [x] Enhanced admin interface
- [x] Docker setup
- [x] Comprehensive documentation
- [ ] Unit tests (TODO)
- [ ] API tests (TODO)

**Frontend: 0% Complete** 🔲
- [ ] Angular project setup
- [ ] RTL support for Arabic
- [ ] Core services (Auth, Product, Cart)
- [ ] UI components
- [ ] Product catalog with filters
- [ ] Product detail with gallery
- [ ] Shopping cart
- [ ] Checkout (COD)
- [ ] Seller dashboard

---

## 📚 Documentation

Comprehensive guides available:

1. **[API_DOCUMENTATION.md](API_DOCUMENTATION.md)** - Complete API reference with cURL examples
2. **[DATABASE_SCHEMA.md](DATABASE_SCHEMA.md)** - Database schema and relationships
3. **[BACKEND_TRANSFORMATION.md](BACKEND_TRANSFORMATION.md)** - Before/after analysis
4. **[QUICKSTART.md](QUICKSTART.md)** - Quick reference guide
5. **[QUICK_REFERENCE_CARD.txt](QUICK_REFERENCE_CARD.txt)** - Command cheat sheet

---

## 🧪 Testing

### Run Backend Tests
```bash
cd backend
docker-compose run --rm web python manage.py test
```

### Manual API Testing
```bash
# Get auth token
curl -X POST http://localhost:8000/api-token-auth/ \
  -H "Content-Type: application/json" \
  -d '{"username":"root","password":"1991"}'

# Create product
curl -X POST http://localhost:8000/api/gr/products/ \
  -H "Authorization: Token YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "product_name": "كبش سردي عمر 18 شهر",
    "breed": "سردي",
    "gender": "ذكر",
    "age_months": 18,
    "weight_kg": 65.5,
    "price": 1500.000,
    "quantity_available": 3,
    "governorate": "باجة",
    "city": "مجاز الباب"
  }'
```

---

## 🚢 Deployment

### Environment Variables
Create `.env` file in `backend/` directory:
```env
DJANGO_SECRET_KEY=your-secret-key
DJANGO_DEBUG=False
DATABASE_URL=postgres://user:pass@host:port/db
CORS_ALLOWED_ORIGINS=https://yourdomain.com
```

### Deploy to Heroku
```bash
heroku create tunisia-livestock
heroku addons:create heroku-postgresql:hobby-dev
git push heroku main
heroku run python manage.py migrate
```

### Deploy with Docker
```bash
docker-compose -f docker-compose.prod.yml up -d
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

### Coding Standards
- Follow PEP 8 for Python
- Use Black for code formatting
- Write docstrings for all functions
- Add unit tests for new features
- Update documentation

---

## 📝 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

---

## 👥 Team

**Developer:** Bilel Nasri  
**Location:** Majaz El Bab, Béja, Tunisia  
**Contact:** bilel@example.com  
**GitHub:** [@biso1991](https://github.com/biso1991)

---

## 🙏 Acknowledgments

- Built for Tunisian farmers and livestock traders
- Inspired by the need for modern agricultural e-commerce in Tunisia
- Special thanks to the Majaz El Bab farming community

---

## 📞 Support

For questions or issues:
- Open an [Issue](https://github.com/biso1991/tunisia-livestock-platform/issues)
- Email: bilel@example.com
- Documentation: See `/docs` folder

---

## 🗺️ Roadmap

### Phase 1: Backend ✅ (Complete)
- [x] Database models
- [x] REST API
- [x] Authentication
- [x] Admin interface
- [x] Documentation

### Phase 2: Frontend (In Progress)
- [ ] Angular setup with RTL
- [ ] Product catalog
- [ ] Shopping cart
- [ ] Checkout flow
- [ ] Seller dashboard

### Phase 3: Features (Planned)
- [ ] WhatsApp integration
- [ ] SMS notifications
- [ ] Email marketing
- [ ] Analytics dashboard
- [ ] Mobile app (React Native)

### Phase 4: Production (Future)
- [ ] Performance optimization
- [ ] SEO optimization
- [ ] Payment gateway integration
- [ ] Shipping API integration
- [ ] Multi-language support

---

## 📊 Stats

- **31 fields** in Product model
- **24 Tunisian governorates** supported
- **6 livestock breeds** classified
- **25+ API endpoints** available
- **10+ filter types** implemented
- **4 images + video** per product
- **95% backend** complete

---

**Made with ❤️ in Tunisia 🇹🇳**

*Transforming livestock trading in Tunisia, one sheep at a time* 🐑
