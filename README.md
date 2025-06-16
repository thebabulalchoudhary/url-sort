# URL Shortener Service

A Laravel-based URL shortening service with multi-company support and role-based access control.

## Features

- **Multi-Company Architecture**: Support for multiple companies with isolated URL management
- **Role-Based Access Control**: Three distinct roles (SuperAdmin, Admin, Member) with specific permissions
- **URL Shortening**: Create and manage shortened URLs with public resolution
- **Invitation System**: Hierarchical user invitation workflow
- **Comprehensive Testing**: Full test coverage for all major functionalities

## Requirements

- PHP 8.1 or higher
- Composer
- Node.js & NPM (for frontend assets)
- SQLite or MySQL database

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/url-shortener.git
cd url-shortener
```

### 2. Install Dependencies

```bash
# Install PHP dependencies
composer install

# Install Node.js dependencies
npm install
```

### 3. Environment Setup

```bash
# Copy environment file
cp .env.example .env

# Generate application key
php artisan key:generate
```

### 4. Database Configuration

#### Option A: SQLite (Recommended for local development)

```bash
# Create SQLite database file
touch database/database.sqlite
```

Update your `.env` file:
```env
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/your/project/database/database.sqlite
```

#### Option B: MySQL

Update your `.env` file with your MySQL credentials:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=url_shortener
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### 5. Database Setup

```bash
# Run migrations
php artisan migrate

# Seed database with SuperAdmin account
php artisan db:seed
```

### 6. Build Frontend Assets

```bash
# Compile assets
npm run build

# Or for development with hot reload
npm run dev
```

### 7. Start the Application

```bash
# Start Laravel development server
php artisan serve
```

The application will be available at `http://localhost:8000`

## Default Credentials

After running the database seeder, you can login with:

- **Email**: `superadmin@example.com`
- **Password**: `password`
- **Role**: SuperAdmin

## User Roles & Permissions

### SuperAdmin
- Can invite Admins to create new companies
- Can view all short URLs across all companies
- **Cannot** create short URLs

### Admin
- Can invite other Admins or Members to their company
- Can create short URLs
- Can view all short URLs created within their company

### Member
- Can create short URLs
- Can only view short URLs they created themselves

## Usage

### Creating Short URLs

1. Login as Admin or Member
2. Navigate to the URL creation form
3. Enter the original URL
4. Click "Create Short URL"
5. The shortened URL will be generated and can be shared publicly

### Managing Users

1. **SuperAdmin**: Can invite new Admins to create companies
2. **Admin**: Can invite other users to join their company

### Viewing URLs

- **SuperAdmin**: Access to all URLs via admin dashboard
- **Admin**: View all company URLs in the dashboard
- **Member**: View only personal URLs in the dashboard

## API Endpoints

### Public Routes
- `GET /{shortCode}` - Redirect to original URL

### Authenticated Routes
- `POST /urls` - Create new short URL
- `GET /urls` - List URLs (filtered by role permissions)
- `POST /invitations` - Send user invitation

## Testing

Run the test suite to verify functionality:

```bash
# Run all tests
php artisan test

# Run specific test features
php artisan test --filter=UrlShorteningTest
php artisan test --filter=AuthorizationTest
php artisan test --filter=InvitationTest
```

### Test Coverage

The test suite covers:
- ✅ Admin and Member can create short URLs
- ✅ SuperAdmin cannot create short URLs
- ✅ Admin can only see URLs from their company
- ✅ Member can only see their own URLs
- ✅ Short URLs are publicly resolvable and redirect correctly

## Development

### File Structure

```
app/
├── Http/Controllers/
│   ├── Auth/
│   ├── UrlController.php
│   ├── InvitationController.php
│   └── DashboardController.php
├── Models/
│   ├── User.php
│   ├── Company.php
│   └── ShortUrl.php
├── Policies/
│   └── UrlPolicy.php
└── Services/
    └── UrlShortenerService.php

database/
├── migrations/
├── seeders/
│   └── DatabaseSeeder.php
└── factories/

tests/
├── Feature/
│   ├── UrlShorteningTest.php
│   ├── AuthorizationTest.php
│   └── InvitationTest.php
└── Unit/
```

### Adding New Features

1. Create feature branch: `git checkout -b feature/new-feature`
2. Implement changes with tests
3. Run test suite: `php artisan test`
4. Submit pull request

## Troubleshooting

### Common Issues

**Database Connection Error**
- Verify database credentials in `.env`
- Ensure database server is running
- Check file permissions for SQLite

**Short URLs Not Resolving**
- Verify web server configuration
- Check `.htaccess` file for proper URL rewriting
- Ensure `APP_URL` is correctly set in `.env`

**Permission Denied Errors**
- Check file permissions: `chmod -R 755 storage bootstrap/cache`
- Ensure web server has write access to storage directories

### Logs

Check application logs for debugging:
```bash
tail -f storage/logs/laravel.log
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes with appropriate tests
4. Submit a pull request

## License

This project is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

## Support

For support and questions, please create an issue in the GitHub repository.
