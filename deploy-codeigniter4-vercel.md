# How to Deploy CodeIgniter 4 to Vercel

## Introduction

This guide will help you deploy your CodeIgniter 4 application to Vercel. Vercel is a cloud platform that provides serverless hosting, which is perfect for CodeIgniter 4 applications.

## Prerequisites

- A CodeIgniter 4 project
- A Vercel account
- Git installed on your computer
- Vercel CLI (optional)

## Step 1: Project Setup

1. Make sure your CodeIgniter 4 project is in a Git repository
2. Ensure your `.env` file is properly configured

## Step 2: Create Vercel Configuration

Create a `vercel.json` file in your project root:

```json
{
    "version": 2,
    "functions": {
      "api/*.php": {
        "runtime": "vercel-php@0.7.3"
      }
    },
    "routes": [
      {
        "src": "/(css|js|images)/(.*)", 
        "dest": "public/$1/$2"
      },
      {
        "src": "/(.*)",
        "dest": "/api/index.php"
      }
    ],
    "env": {
        "APP_BASE_URL": "your-domain.vercel.app",
        "CI_ENVIRONMENT": "production"
    }
}
```

## Step 3: Create API Entry Point

1. Create an `api` directory in your project root:
   ```bash
   mkdir api
   ```

2. Create `api/index.php` with this simple content:
   ```php
   <?php
   require __DIR__.'/../public/index.php';
   ```

## Step 4: Configure Cache

Since Vercel's environment is read-only, modify `app/Config/Cache.php` to use `/tmp` for cache storage:

```php
<?php
public array $file = [
    'storePath' => '/tmp',
    'mode'      => 0640,
];
```

## Step 5: Environment Variables

1. Go to your Vercel project settings
2. Add these important environment variables:
   - Database credentials
   - Encryption keys
   - Other sensitive configuration from your `.env` file

## Step 6: Deploy

### Option 1: Using Vercel CLI

```bash
# Install Vercel CLI
npm i -g vercel

# Login to Vercel
vercel login

# Deploy
vercel
```

### Option 2: Using Vercel Dashboard

1. Push your code to GitHub/GitLab/Bitbucket
2. Import your repository in Vercel dashboard
3. Click "Deploy"

## Common Issues

### Database Connection
- Verify database credentials in Vercel environment variables
- Ensure database is accessible from Vercel's servers

### File Uploads
- Use external storage (like AWS S3)
- Configure proper permissions

### Performance
- Enable CodeIgniter 4 caching
- Use Vercel's edge caching
- Optimize database queries

## Best Practices

1. Use environment variables for sensitive data
2. Implement proper error handling
3. Keep your CodeIgniter 4 installation updated
4. Regular backups of your database

## Additional Resources

- [CodeIgniter 4 Documentation](https://codeigniter.com/user_guide/)
- [Vercel Documentation](https://vercel.com/docs)
- [PHP on Vercel](https://vercel.com/docs/runtimes#official-runtimes/php) 