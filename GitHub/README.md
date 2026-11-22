
## 3. GitHub – HTTP 403 Permission Denied

**Problem:**

GitHub rejected an authenticated Git operation while working with Personal Access Tokens (PAT).

**Error:**

```text
remote: Permission to repository denied.
fatal: unable to access repository:
The requested URL returned error: 403
```

**Root Cause:**

Possible causes include:
- PAT missing required repository permissions.
- Authenticated account does not have repository access.
- Incorrect or outdated stored credentials.
- Organization access restrictions.

**Solution / Troubleshooting:**

1. Verify repository access.
2. Check PAT permissions.
3. Confirm the correct GitHub account is authenticated.
4. Update stored Git credentials if necessary.

**Useful Commands:**

```bash
git remote -v
git status
git config --list
git push origin main
```

**Learning:** GitHub authentication and repository authorization are different. A valid token does not automatically grant access to every repository.
