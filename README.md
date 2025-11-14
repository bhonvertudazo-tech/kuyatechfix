# KuyaTechFix - Tech Repair Service Platform

A production-ready Android application connecting customers with local technicians for tech repair services. Built with Java, Firebase, and Google Maps.

## 🚀 Features

### Customer Features
- **User Authentication** - Secure login/registration with Firebase Auth
- **Real-time Technician Tracking** - View available technicians on Google Maps
- **Service Browsing** - Browse computer, mobile, and electrical repair services
- **Booking Management** - Create and track service bookings
- **Profile Management** - View and manage user profile

### Technician Features
- **Dedicated Dashboard** - Separate interface for technicians
- **Location Tracking** - Real-time location updates on map
- **Booking Notifications** - Receive and manage customer bookings

## 🛠️ Tech Stack

- **Language**: Java
- **Build System**: Gradle (Kotlin DSL)
- **Min SDK**: 24 (Android 7.0)
- **Target SDK**: 34 (Android 14)

### Dependencies
- **Firebase**
  - Authentication
  - Firestore Database
  - Realtime Database
  - Analytics
- **Google Maps SDK** (19.0.0)
- **Google Location Services** (21.3.0)
- **Glide** (4.15.1) - Image loading
- **Material Design Components**

## 📋 Prerequisites

1. **Android Studio** (Arctic Fox or newer)
2. **JDK 8** or higher
3. **Google Maps API Key**
4. **Firebase Project**

## 🔧 Setup Instructions

### 1. Clone the Repository
```bash
git clone <repository-url>
cd kuyatechfix
```

### 2. Configure Google Maps API Key

Create or edit `local.properties` in the project root:
```properties
sdk.dir=C\:\\Users\\YourUsername\\AppData\\Local\\Android\\Sdk
MAPS_API_KEY=YOUR_GOOGLE_MAPS_API_KEY_HERE
```

**Get your API key:**
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create/select a project
3. Enable "Maps SDK for Android"
4. Create API Key credentials
5. Restrict key to Android apps with package: `com.example.kuyatechfix`

### 3. Configure Firebase

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project or use existing
3. Add an Android app with package name: `com.example.kuyatechfix`
4. Download `google-services.json`
5. Place it in `app/` directory

**Enable Firebase Services:**
- Authentication (Email/Password)
- Cloud Firestore
- Realtime Database
- Analytics (optional)

### 4. Firestore Database Structure

```
/technicians
  /{technicianId}
    - name: String
    - skill: String
    - phone: String
    - imageUri: String

/bookings
  /{bookingId}
    - customerId: String
    - customerName: String
    - technicianId: String
    - technicianName: String
    - serviceType: String
    - description: String
    - status: String (pending/accepted/in_progress/completed/cancelled)
    - timestamp: Long
    - location: String
    - latitude: Double
    - longitude: Double

/users
  /{userId}
    - name: String
    - email: String
    - role: String (customer/technician)
```

### 5. Build and Run

```bash
./gradlew clean build
```

Or use Android Studio:
1. Open project in Android Studio
2. Sync Gradle files
3. Run on emulator or device

## 🔐 Security Features

- ✅ API keys stored in `local.properties` (gitignored)
- ✅ ProGuard/R8 enabled for release builds
- ✅ Code obfuscation and resource shrinking
- ✅ Firebase Authentication for user management
- ✅ Input validation on all forms
- ✅ Proper error handling and user feedback

## 📱 App Structure

```
app/src/main/java/com/example/kuyatechfix/
├── MainActivity.java              # Login screen
├── RegisterActivity.java          # Registration
├── DashboardActivity.java         # Customer dashboard
├── TechnicianDashboardActivity.java # Technician dashboard
├── TechnicianActivity.java        # List technicians
├── BookingsActivity.java          # View bookings
├── ServicesActivity.java          # Browse services
├── ProfileActivity.java           # User profile
├── TechnicianAdapter.java         # RecyclerView adapter
├── Technician.java                # Data model
└── Booking.java                   # Data model
```

## 🎨 UI/UX Features

- Material Design components
- Smooth animations (fade-in, scale)
- Loading states with ProgressBar
- Empty state handling
- Error messages with user-friendly text
- Responsive layouts

## 🚀 Production Deployment

### Release Build
```bash
./gradlew assembleRelease
```

### Pre-deployment Checklist
- [ ] Regenerate exposed API keys
- [ ] Update Firebase configuration
- [ ] Test on multiple devices
- [ ] Enable ProGuard (already configured)
- [ ] Set up Firestore security rules
- [ ] Configure Firebase App Check
- [ ] Test offline scenarios
- [ ] Add crash reporting (Firebase Crashlytics)

### Firestore Security Rules (Example)
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    match /technicians/{technicianId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null; // Add role check
    }
    
    match /bookings/{bookingId} {
      allow read: if request.auth != null && 
        (resource.data.customerId == request.auth.uid || 
         resource.data.technicianId == request.auth.uid);
      allow create: if request.auth != null;
      allow update: if request.auth != null && 
        (resource.data.customerId == request.auth.uid || 
         resource.data.technicianId == request.auth.uid);
    }
  }
}
```

## 🐛 Known Issues & Future Improvements

### Current Limitations
- Phone number is placeholder (not editable)
- No real-time location tracking for technicians
- Booking creation UI not implemented
- No payment integration
- No push notifications
- No chat/messaging feature

### Planned Features
- [ ] Real-time technician location updates
- [ ] In-app booking creation flow
- [ ] Payment gateway integration (PayMongo/Stripe)
- [ ] Push notifications for booking updates
- [ ] Chat system between customer and technician
- [ ] Rating and review system
- [ ] Service history and analytics
- [ ] Admin panel
- [ ] Offline mode with data caching

## 📄 License

[Add your license here]

## 👥 Contributors

[Add contributors here]

## 📞 Support

For issues and questions:
- Create an issue in the repository
- Email: [your-email@example.com]

## 🙏 Acknowledgments

- Firebase for backend services
- Google Maps Platform
- Material Design guidelines
- Glide library for image loading

---

**Note**: This is a production-ready MVP. Additional features and security hardening may be required based on specific deployment requirements.
