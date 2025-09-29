# Simple Chat - Firebase Flutter App

A modern Flutter chat application built with Clean Architecture, BLoC, Firebase, and Material 3 design.

## Features

- 🔐 **Authentication**: Email/password and Google Sign-In
- 💬 **Real-time Chat**: Text and audio messages
- 🌍 **Multi-language Support**: English, Russian, Ukrainian
- 🎨 **Material 3 Design**: Modern UI with dark/light themes
- 🏗️ **Clean Architecture**: Maintainable and testable code structure
- 📱 **Cross-platform**: iOS, Android, Web, macOS, Windows

## Architecture

The project follows Clean Architecture principles:

```
lib/
├── core/                    # Core functionality
│   ├── di/                 # Dependency injection
│   ├── theme/              # App theming
│   ├── utils/              # Utility functions
│   └── constants/          # App constants
├── features/               # Feature modules
│   ├── auth/              # Authentication feature
│   │   ├── data/          # Data layer
│   │   ├── domain/        # Domain layer
│   │   └── presentation/  # Presentation layer
│   └── chat/              # Chat feature
│       ├── data/          # Data layer
│       ├── domain/        # Domain layer
│       └── presentation/  # Presentation layer
└── l10n/                  # Localization files
```

## Setup Instructions

### 1. Prerequisites

- Flutter SDK (3.9.2 or higher)
- Dart SDK
- Firebase project
- Android Studio / VS Code

### 2. Firebase Setup

1. Create a new Firebase project at [Firebase Console](https://console.firebase.google.com/)
2. Enable Authentication with Email/Password and Google Sign-In
3. Create a Firestore database
4. Enable Firebase Storage
5. Download configuration files:
   - `google-services.json` for Android (place in `android/app/`)
   - `GoogleService-Info.plist` for iOS (place in `ios/Runner/`)

### 3. Update Firebase Configuration

Update `lib/firebase_options.dart` with your Firebase project configuration:

```dart
static const FirebaseOptions web = FirebaseOptions(
  apiKey: 'your-web-api-key',
  appId: 'your-web-app-id',
  messagingSenderId: 'your-sender-id',
  projectId: 'your-project-id',
  authDomain: 'your-project-id.firebaseapp.com',
  storageBucket: 'your-project-id.appspot.com',
);
```

### 4. Install Dependencies

```bash
cd app
flutter pub get
```

### 5. Generate Code

```bash
dart run build_runner build --delete-conflicting-outputs
```

### 6. Run the App

```bash
flutter run
```

## Project Structure

### Core Module
- **DI**: Dependency injection setup with `get_it` and `injectable`
- **Theme**: Material 3 theming with light/dark mode support
- **Utils**: Date formatting, validation, and other utilities
- **Constants**: App-wide constants and configuration

### Auth Feature
- **Entities**: User model with `freezed`
- **Repositories**: Authentication repository interface
- **Use Cases**: Sign in, sign up, sign out operations
- **BLoC**: Authentication state management
- **UI**: Login page with email/password and Google Sign-In

### Chat Feature
- **Entities**: Message model with support for text and audio
- **Repositories**: Chat repository interface
- **Use Cases**: Send messages, load messages, upload audio
- **BLoC**: Chat state management
- **UI**: Chat interface with message bubbles and audio player

## Dependencies

### Core
- `flutter_bloc` - State management
- `freezed` - Immutable data classes
- `get_it` + `injectable` - Dependency injection
- `go_router` - Navigation

### Firebase
- `firebase_core` - Firebase initialization
- `firebase_auth` - Authentication
- `cloud_firestore` - Database
- `firebase_storage` - File storage

### UI/UX
- `cached_network_image` - Image caching
- `url_launcher` - Link handling
- `flutter_localizations` - Internationalization

### Audio
- `record` - Audio recording
- `just_audio` - Audio playback

## Development

### Code Generation

After making changes to models or DI configuration:

```bash
dart run build_runner build --delete-conflicting-outputs
```

### Testing

```bash
flutter test
```

### Linting

```bash
flutter analyze
```

## Firebase Security Rules

### Firestore Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can read/write their own user document
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Messages in chats
    match /chats/{chatId}/messages/{messageId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### Storage Rules

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /audio_messages/{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run tests and linting
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.