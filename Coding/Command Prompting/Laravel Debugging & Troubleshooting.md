# Cache Clearing Commands

### Configuration Cache

```bash
# Clear configuration cache
php artisan config:clear

# Create configuration cache
php artisan config:cache

# Note: Always clear before caching to avoid conflicts
php artisan config:clear && php artisan config:cache
```

### Route Cache

```bash
# Clear route cache
php artisan route:clear

# Create route cache
php artisan route:cache

# List all routes
php artisan route:list

# Filter routes by name
php artisan route:list --name=user

# Filter routes by URI
php artisan route:list --path=api

# Filter routes by method
php artisan route:list --method=POST
```

### View Cache

```bash
# Clear compiled views
php artisan view:clear

# Cache views
php artisan view:cache
```

### Application Cache

```bash
# Clear application cache
php artisan cache:clear

# Clear specific cache store
php artisan cache:clear --store=redis

# Clear cache and forget all keys
php artisan cache:forget <key>
```

### Event & Queue Cache

```bash
# Clear event cache
php artisan event:clear

# Cache events
php artisan event:cache

# Clear queue failed jobs
php artisan queue:clear
```

### Complete Cache Clear

```bash
# Run all cache clearing commands
php artisan config:clear
php artisan cache:clear
php artisan route:clear
php artisan view:clear
php artisan event:clear

# One-liner
php artisan config:clear && php artisan cache:clear && php artisan route:clear && php artisan view:clear
```

---

## Composer Commands

### Autoload Issues

```bash
# Regenerate autoload files
composer dump-autoload

# Optimize autoload (faster)
composer dump-autoload -o

# Optimize for production
composer dump-autoload --optimize --no-dev
```

### Dependency Management

```bash
# Install dependencies
composer install

# Install without dev dependencies
composer install --no-dev

# Update all dependencies
composer update

# Update specific package
composer update vendor/package

# Remove package
composer remove vendor/package

# Require new package
composer require vendor/package

# Require dev package
composer require --dev vendor/package
```

---

## PHP Configuration Issues

### Memory Limit Error

**Error:**

```
Allowed memory size of 536870912 bytes exhausted
```

**Solution 1: Edit php.ini**

```ini
memory_limit = 1024M
# or
memory_limit = 2048M
# or unlimited (not recommended)
memory_limit = -1
```

**Find php.ini location:**

```bash
php --ini

# Or
php -i | grep php.ini

# On Ubuntu/Debian
sudo nano /etc/php/8.2/cli/php.ini
sudo nano /etc/php/8.2/fpm/php.ini
sudo nano /etc/php/8.2/apache2/php.ini
```

**Solution 2: Runtime (temporary)**

```php
// In your PHP script
ini_set('memory_limit', '1024M');

// In Laravel AppServiceProvider
public function register()
{
    ini_set('memory_limit', '1024M');
}
```

**Solution 3: Composer memory limit**

```bash
# For single command
COMPOSER_MEMORY_LIMIT=-1 composer update

# Or set permanently
export COMPOSER_MEMORY_LIMIT=-1
```

---

### Maximum Execution Time Error

**Error:**

```
Maximum execution time of 60 seconds exceeded
```

**Solution 1: Edit php.ini**

```ini
max_execution_time = 300
# or unlimited (not recommended)
max_execution_time = 0
```

**Solution 2: Runtime in Laravel**

```php
// In AppServiceProvider.php
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    public function register()
    {
        ini_set('max_execution_time', 300);
    }

    public function boot()
    {
        //
    }
}
```

**Solution 3: For specific routes/controllers**

```php
// In controller method
public function longRunningTask()
{
    set_time_limit(300); // 5 minutes
    
    // Your code here
}

// Or unlimited (use with caution)
public function veryLongTask()
{
    set_time_limit(0);
    
    // Your code here
}
```

**Solution 4: For Artisan commands**

```php
// In your command class
public function handle()
{
    set_time_limit(0);
    
    // Your command logic
}
```

---

## Database Issues

### Migration Errors

```bash
# Rollback last migration
php artisan migrate:rollback

# Rollback all migrations
php artisan migrate:reset

# Rollback and re-run all migrations
php artisan migrate:refresh

# Refresh and seed
php artisan migrate:refresh --seed

# Drop all tables and migrate
php artisan migrate:fresh

# Fresh migrate with seed
php artisan migrate:fresh --seed

# Check migration status
php artisan migrate:status
```

### Database Schema

```bash
# Dump database schema (for version control)
php artisan schema:dump

# Dump schema and prune old migrations
php artisan schema:dump --prune
```

### Database Connection Issues

```bash
# Test database connection
php artisan db:show

# Show database tables
php artisan db:table <table_name>

# Monitor database
php artisan db:monitor
```

---

## Queue & Job Issues

### Queue Commands

```bash
# Process queue jobs
php artisan queue:work

# Process specific queue
php artisan queue:work --queue=high,default

# Process one job
php artisan queue:work --once

# Stop gracefully after current job
php artisan queue:work --stop-when-empty

# Restart queue workers
php artisan queue:restart

# View failed jobs
php artisan queue:failed

# Retry failed job
php artisan queue:retry <job_id>

# Retry all failed jobs
php artisan queue:retry all

# Forget failed job
php artisan queue:forget <job_id>

# Clear all failed jobs
php artisan queue:flush
```

---

## Permission Issues

### Storage & Cache Permissions

```bash
# Fix storage and cache permissions
sudo chown -R www-data:www-data storage bootstrap/cache

# Or with your user
sudo chown -R $USER:www-data storage bootstrap/cache

# Set proper permissions
sudo chmod -R 775 storage bootstrap/cache

# On development
chmod -R 777 storage bootstrap/cache  # Not recommended for production!
```

---

## Performance Optimization

### Optimize for Production

```bash
# Clear all caches first
php artisan optimize:clear

# Then optimize
php artisan optimize

# This runs:
# - config:cache
# - route:cache
# - view:cache
# - event:cache
```

### Individual Optimizations

```bash
# Cache config
php artisan config:cache

# Cache routes
php artisan route:cache

# Cache views
php artisan view:cache

# Cache events
php artisan event:cache

# Optimize composer
composer install --optimize-autoloader --no-dev
```

---

## Debug Mode & Logging

### Enable Debug Mode

```env
# .env file
APP_DEBUG=true
APP_ENV=local
LOG_CHANNEL=stack
LOG_LEVEL=debug
```

### View Logs

```bash
# View real-time logs
tail -f storage/logs/laravel.log

# View last 100 lines
tail -n 100 storage/logs/laravel.log

# Clear logs
truncate -s 0 storage/logs/laravel.log

# Or
> storage/logs/laravel.log
```

### Laravel Telescope (Development)

```bash
# Install Telescope
composer require laravel/telescope --dev

# Publish assets
php artisan telescope:install

# Migrate
php artisan migrate

# Clear Telescope data
php artisan telescope:clear

# Prune old records
php artisan telescope:prune
```

### Debug pretty dump die
```php
dd($data->toJson(JSON_PRETTY_PRINT))
or
dd(json_encode($data, JSON_PRETTY_PRINT));
```

---

## Common Errors & Solutions

### "Class not found" Error

```bash
# Solution:
composer dump-autoload
php artisan clear-compiled
php artisan cache:clear
```

### "No application encryption key"

```bash
# Solution:
php artisan key:generate
```

### "419 Page Expired" (CSRF)

```bash
# Clear cache and sessions
php artisan cache:clear
php artisan session:flush
php artisan config:clear
```

### "Symfony\Component\Debug\Exception\FatalErrorException"

```bash
# Check:
# 1. PHP version compatibility
php -v

# 2. Required extensions
php -m

# 3. Composer dependencies
composer install

# 4. Clear caches
php artisan optimize:clear
```

### Database Connection Refused

```bash
# Check .env file
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database
DB_USERNAME=your_username
DB_PASSWORD=your_password

# Clear config cache
php artisan config:clear

# Test connection
php artisan db:show
```

### Storage Link Missing

```bash
# Create storage symlink
php artisan storage:link

# Verify link
ls -la public/storage
```

---

## Useful Artisan Commands

### Application Info

```bash
# Show application info
php artisan about

# List all commands
php artisan list

# Get help for command
php artisan help <command>

# Run in maintenance mode
php artisan down

# Bring back up
php artisan up

# Custom maintenance mode message
php artisan down --message="Upgrading database" --retry=60
```

### Code Generation

```bash
# Make model with migration, factory, seeder, controller
php artisan make:model Product -mfsc

# Make model with all resources
php artisan make:model Product -a

# Make controller with resource methods
php artisan make:controller ProductController --resource

# Make API controller
php artisan make:controller API/ProductController --api

# Make request
php artisan make:request StoreProductRequest

# Make middleware
php artisan make:middleware CheckAge

# Make migration
php artisan make:migration create_products_table

# Make seeder
php artisan make:seeder ProductSeeder

# Make factory
php artisan make:factory ProductFactory

# Make policy
php artisan make:policy ProductPolicy

# Make observer
php artisan make:observer ProductObserver --model=Product
```

---

## Testing

### Run Tests

```bash
# Run all tests
php artisan test

# Run specific test
php artisan test --filter=UserTest

# Run with coverage
php artisan test --coverage

# Parallel testing
php artisan test --parallel

# Create test
php artisan make:test UserTest

# Create unit test
php artisan make:test UserTest --unit
```

---

## Additional Issues

### WiFi Intel 201 Fix (Ubuntu/Linux)

**Issue:** WiFi not working after sleep/hibernate

**Solution:**

```bash
# Option 1: Restart laptop
shutdown now
# Then press power button to start

# Option 2: Through BIOS
shutdown now
# Press appropriate key to enter BIOS during boot
# Exit BIOS and continue boot

# Option 3: Reload WiFi driver
sudo modprobe -r iwlwifi
sudo modprobe iwlwifi

# Option 4: Restart NetworkManager
sudo systemctl restart NetworkManager
```

---

## Quick Troubleshooting Checklist

When things go wrong:

```bash
# 1. Clear all caches
php artisan optimize:clear

# 2. Regenerate autoload
composer dump-autoload

# 3. Check .env file
cat .env

# 4. Check logs
tail -f storage/logs/laravel.log

# 5. Fix permissions
chmod -R 775 storage bootstrap/cache

# 6. Restart services
sudo systemctl restart apache2
# or
sudo systemctl restart nginx
sudo systemctl restart php8.2-fpm

# 7. Check PHP version
php -v

# 8. Check database connection
php artisan db:show
```

---

## Production Deployment Checklist

```bash
# 1. Enable production mode
# .env
APP_ENV=production
APP_DEBUG=false

# 2. Clear development caches
php artisan optimize:clear

# 3. Install dependencies (no dev)
composer install --optimize-autoloader --no-dev

# 4. Run migrations
php artisan migrate --force

# 5. Optimize for production
php artisan optimize

# 6. Create storage link
php artisan storage:link

# 7. Set proper permissions
chown -R www-data:www-data .
chmod -R 755 .
chmod -R 775 storage bootstrap/cache

# 8. Restart services
sudo systemctl restart php8.2-fpm
sudo systemctl restart nginx
```