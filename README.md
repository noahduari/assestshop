# QuickShop Marketplace

A premium digital marketplace for games, templates, and assets with Firebase authentication.

## Features

- 🛒 **Digital Marketplace** - Buy and sell game templates, premium games, and digital assets
- 🔐 **Firebase Authentication** - Google and email/password sign-in
- 🌍 **Regional Pricing** - Automatic currency conversion based on location
- 📱 **Responsive Design** - Works on all devices
- 🎨 **Modern UI** - Dark cyberpunk theme with smooth animations

## Technologies Used

- HTML5, CSS3, JavaScript (ES6+)
- Bootstrap 5
- Firebase (Auth & Firestore)
- Font Awesome & Bootstrap Icons

## Getting Started

### Prerequisites
- Node.js (for local development)
- Firebase project

### Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/quickshop-marketplace.git
cd quickshop-marketplace
```

2. Install dependencies:
```bash
npm install
```

3. Set up Firebase:
   - Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
   - Enable Authentication (Google & Email/Password)
   - Set up Firestore Database
   - Update the Firebase config in `index.html`

4. Run locally:
```bash
npm run dev
```

## Deployment

### Vercel (Recommended)
1. Push to GitHub
2. Connect to Vercel
3. Deploy automatically

### Firebase Hosting
```bash
firebase init hosting
firebase deploy
```

## Project Structure

```
quickshop-marketplace/
├── index.html          # Main HTML file
├── package.json        # Node.js dependencies
├── README.md          # This file
└── vercel.json        # Vercel configuration
```

## Firebase Setup

1. **Authentication**: Enable Google and Email/Password providers
2. **Authorized Domains**: Add your deployment domain
3. **Firestore Rules**: Configure security rules for user data
4. **Project Name**: Set a clean display name for Google sign-in

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.