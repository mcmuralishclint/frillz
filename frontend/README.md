# Frontend Build - Static Files

## Deployment to cPanel

1. **Upload Files**
   - Upload all files from this directory to your cPanel's `public_html` folder
   - You can use cPanel File Manager or FTP client

2. **Configure API Endpoint**
   - Edit `config.js` and update the `API_URL` to point to your backend server
   - If backend is on same domain: use `/api`
   - If backend is on different domain: use full URL like `https://api.yourdomain.com`

3. **Verify Deployment**
   - Visit your domain in a browser
   - Check browser console for any errors
   - Ensure all assets load correctly

## Files Included

- `index.html` - Main entry point
- `assets/` - All JavaScript, CSS, and images
- `.htaccess` - Apache configuration for SPA routing
- `config.js` - Frontend configuration (update this!)

## Troubleshooting

- **404 errors on refresh**: Ensure `.htaccess` is uploaded and mod_rewrite is enabled
- **API errors**: Check that `config.js` has the correct backend URL
- **Assets not loading**: Verify all files uploaded correctly
