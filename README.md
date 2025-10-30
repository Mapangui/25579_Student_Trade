# 25579-MedTterm-WebTech

# 🎓 CampusTrade - Student Marketplace Platform

## 📋 Project Context & Why I Chose This Subject

### 🎯 **Why CampusTrade?**
I chose to create **CampusTrade** because as a student myself, I understand the daily challenges we face in accessing affordable educational resources. Traditional e-commerce platforms like Amazon or eBay don't address the specific needs of students:

- **Financial Constraints**: Students need budget-friendly options for textbooks and supplies
- **Campus-Specific Needs**: University materials, notes, and tutoring are unique to student life
- **Community Focus**: Students prefer trading within their academic community for trust and relevance
- **Location Matters**: Campus location affects what items and services are available

### 💡 **Real Student Problems Solved**
- 📚 **Expensive Textbooks** - Buy/sell used books at student-friendly prices
- 💻 **Tech Equipment** - Affordable laptops and calculators for coursework
- 🎓 **Study Materials** - Share notes, summaries, and study guides
- 👨‍🏫 **Peer Tutoring** - Find tutors within the same university
- 👥 **Study Groups** - Connect with classmates for collaborative learning

## 🏗️ Project Structure

### 📁 **Package Architecture**
```
src/main/java/rw/ac/campustrade/
├── model/              # Entity Classes
│   ├── Student.java
│   ├── Location.java   # Rwanda administrative structure
│   ├── Listing.java    # Items/Services for sale
│   ├── Category.java
│   ├── StudentProfile.java
│   └── StudyGroup.java
├── repository/         # Spring Data JPA Repositories
│   ├── StudentRepository.java
│   ├── LocationRepository.java
│   ├── ListingRepository.java
│   ├── CategoryRepository.java
│   ├── StudentProfileRepository.java
│   └── StudyGroupRepository.java
├── service/           # Business Logic Layer
│   └── StudentService.java
├── controller/        # REST API Endpoints
│   ├── StudentController.java
│   ├── LocationController.java
│   ├── ListingController.java
│   └── CategoryController.java
└── config/
    └── DataInitializer.java  # Sample data loader
```

### 🗄️ **Database Schema**
```
students ↔ locations (Many-to-One)
students ↔ student_profiles (One-to-One)  
students ↔ study_groups (Many-to-Many)
students ↔ listings (One-to-Many as seller)
listings ↔ categories (Many-to-One)
```

## ⚙️ Technical Implementation

### ✅ **Requirements Met**

#### 1. **5+ Entities with CRUD**
- ✅ Student (Complete CRUD)
- ✅ Location (Complete CRUD) 
- ✅ Listing (Complete CRUD)
- ✅ Category (Complete CRUD)
- ✅ StudentProfile (Complete CRUD)
- ✅ StudyGroup (Complete CRUD)

#### 2. **JPA Repository Methods**
```java
// Derived queries
findByEmail(), findByStudentId(), existsByEmail()

// Location-based queries  
findByLocationProvinceCode(), findByLocationProvince()

// Pagination & Sorting
findByUniversity(String university, Pageable pageable)
findByUniversity(String university, Sort sort)

// Search functionality
findByFirstNameContainingOrLastNameContaining()
findByTitleContainingOrDescriptionContaining()
```

#### 3. **Rwandan Location Structure**
```java
Province → District → Sector → Cell → Village
Example: "Kigali City" → "Gasabo" → "Remera" → "Rukiri I" → "Amahoro"
```

#### 4. **Student-Location Relationship API**
```java
// Get students by province code
GET /api/students/province-code/{code}

// Get students by province name  
GET /api/students/province/{name}

// Get locations by province
GET /api/locations/province/{name}
```

#### 5. **All Three Relationship Types**
- **One-to-One**: Student ↔ StudentProfile
- **One-to-Many**: Student → Listings, Location → Students
- **Many-to-Many**: Student ↔ StudyGroup

## 🐛 Issues Faced & Solutions

### 🔧 **Major Challenges**

#### 1. **Lombok Compatibility Issues**
**Problem**: Eclipse IDE had conflicts with Lombok annotations
```java
// BROKEN: Lombok annotations causing compilation errors
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Student { ... }
```
**Solution**: Removed Lombok and implemented manual getters/setters
```java
// FIXED: Manual implementation
public class Student {
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    // ... all other getters/setters
}
```

#### 2. **Repository Visibility**
**Problem**: Repository interfaces weren't accessible
**Solution**: Made all repositories `public` and added `@Repository`

#### 3. **Constructor Mismatches**
**Problem**: Wrong constructor parameters during data initialization
**Solution**: Created proper constructors matching entity fields

#### 4. **API Endpoint Mapping**
**Problem**: 404 errors due to missing controller mappings
```java
// WRONG: Missing endpoint mapping
// FIXED: Added proper request mapping
@GetMapping("/province/{province}")
public List<Student> getStudentsByProvince(@PathVariable String province)
```

#### 5. **Pagination Issues**
**Problem**: `PageRequest` vs `Pageable` type confusion
**Solution**: Use `Pageable` in repository, `PageRequest.of()` in controller

## 🚀 API Endpoints Summary

### 👥 **Student Management**
```
GET    /api/students                    # Get all students
POST   /api/students                    # Create new student
GET    /api/students/{id}              # Get student by ID
PUT    /api/students/{id}              # Update student
DELETE /api/students/{id}              # Delete student
GET    /api/students/province/{name}   # Students by province
GET    /api/students/province-code/{code} # Students by province code
```

### 📍 **Location Management**
```
GET    /api/locations                   # Get all locations
POST   /api/locations                   # Create new location
GET    /api/locations/{id}             # Get location by ID
GET    /api/locations/province/{name}  # Locations by province
GET    /api/locations/province-code/{code} # Locations by province code
```

### 🛒 **Listing Marketplace**
```
GET    /api/listings                    # Get all listings
POST   /api/listings                    # Create new listing
GET    /api/listings/{id}              # Get listing by ID
GET    /api/listings/category/{id}     # Listings by category
GET    /api/listings/available         # Available listings
GET    /api/listings/price-range       # Listings by price range
GET    /api/listings/search            # Search listings
GET    /api/listings/search-page       # Search with pagination
```

## 🎯 Unique Features for Students

### 🏫 **Campus-Centric Design**
- University-specific filtering
- Student ID verification system
- Campus location-based services
- Peer-to-peer academic support

### 📚 **Academic Focus**
- Textbook trading marketplace
- Study notes sharing platform
- Tutoring services between students
- Study group coordination

### 🇷🇼 **Rwanda-Specific**
- Complete Rwandan administrative structure
- Local university integration
- Rwandan student needs addressed
- Local currency (RWF) pricing

## 🔮 Future Enhancements

### 🚀 **Planned Features**
- 🔐 **Authentication & Authorization**
- 💬 **Real-time messaging between students**
- ⭐ **Rating and review system**
- 🔔 **Notification system for price drops**
- 📱 **Mobile application**
- 💳 **Payment integration for Rwandan banks**
- 🎓 **University verification system**

### 🛠️ **Technical Improvements**
- ✅ **Input validation and error handling**
- ✅ **API documentation with Swagger**
- ✅ **Unit and integration testing**
- ✅ **Docker containerization**
- ✅ **Performance optimization**

## 📊 Sample Data Loaded

The application automatically initializes with:
- 🏙️ **3 Locations** (Kigali Gasabo, Kigali Kicukiro, Southern Huye)
- 📚 **4 Categories** (Textbooks, Electronics, Tutoring, Study Notes)
- 👥 **3 Students** with different majors and universities
- 📝 **2 Student Profiles** with ratings and interests
- 🛒 **3 Listings** (Textbook, Laptop, Tutoring Service)

## 🎓 Conclusion

**CampusTrade** successfully addresses the unique marketplace needs of Rwandan students by providing a platform specifically designed for academic resource sharing, peer-to-peer services, and community building within educational institutions. The project demonstrates comprehensive Spring Boot implementation while solving real-world problems faced by students daily.

The platform's focus on affordability, trust, and campus community makes it a valuable tool for enhancing the student experience in Rwanda's educational ecosystem.
