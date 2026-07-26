# Security Guidelines for Verdikt

## Environment Variables

All sensitive information should be stored in environment variables, not in source code.

### Setup Instructions

1. Copy `.env.example` to `.env.local`:
   ```bash
   cp .env.example .env.local
   ```

2. Fill in your actual values in `.env.local`:
   ```
   VITE_FIREBASE_API_KEY=your_actual_key
   VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
   # ... etc
   ```

3. **Never commit `.env.local`** - it's already in `.gitignore`

### Environment Variables Reference

| Variable | Purpose | Sensitivity |
|----------|---------|-------------|
| `VITE_FIREBASE_API_KEY` | Firebase API Key | 🔴 HIGH - Keep secret |
| `VITE_FIREBASE_AUTH_DOMAIN` | Firebase Auth Domain | 🟡 MEDIUM - Can be public |
| `VITE_FIREBASE_PROJECT_ID` | Firebase Project ID | 🟡 MEDIUM - Can be public |
| `VITE_SUPPORT_EMAIL` | Support contact email | 🟢 LOW - Public info |

## API Keys Management

### Google PageSpeed API Key
- Store in browser's localStorage only after user input
- Never hardcode in source
- Users enter via "API Settings" modal

### Gemini API Key
- Store in browser's localStorage only after user input
- Never commit to repository
- Users configure via settings UI

### OpenAI API Key
- Store in browser's localStorage only after user input
- Best practice: Use a backend proxy (not implemented yet)
- Never expose to client-side code in production

## Best Practices

1. ✅ Use environment variables via `import.meta.env.VARIABLE_NAME`
2. ✅ Use `.env.local` for local development (ignored by git)
3. ✅ Use `.env.example` to document required variables
4. ✅ Enable GitHub's secret scanning
5. ❌ Never hardcode secrets in HTML/JavaScript
6. ❌ Never commit `.env` or `.env.local`
7. ❌ Never expose email addresses publicly in code

## Deployed Application

When deploying to production:

1. Set environment variables via your hosting platform:
   - Vercel: Project Settings → Environment Variables
   - Netlify: Site Settings → Build & Deploy → Environment
   - AWS: Environment variables in Lambda/Amplify
   - Docker: Use `--env-file` or docker-compose

2. Ensure `.env.local` is in `.gitignore`

3. Use GitHub branch protection rules to prevent accidental commits

## Detecting Leaks

If credentials were accidentally committed:

1. **Rotate** the exposed credentials immediately
2. **Remove** from git history:
   ```bash
   git filter-branch --tree-filter 'rm -f .env.local' HEAD
   ```
3. **Force push** (only if not public):
   ```bash
   git push origin --force
   ```
4. **Notify** any affected platforms of potential compromise

## References

- [OWASP: Secrets Management](https://owasp.org/www-project-top-10/)
- [GitHub: Secret Scanning](https://docs.github.com/en/code-security/secret-scanning)
- [Vite: Environment Variables](https://vitejs.dev/guide/env-and-mode.html)
