# LinkedIn OAuth Scopes Location

## Primary Definition

The LinkedIn OAuth scopes are defined in the OAuth connection route:

**File:** `app/api/auth/connect/linkedin/route.ts`  
**Line:** 26

```typescript
const scope = 'openid profile email w_member_social r_organization_admin w_organization_social';
```

## Scope Breakdown

The application requests the following LinkedIn OAuth scopes:

1. **`openid`** - OpenID Connect authentication
2. **`profile`** - Access to basic profile information
3. **`email`** - Access to user's email address
4. **`w_member_social`** - Permission to post content on behalf of the user (personal profile)
5. **`r_organization_admin`** - Permission to read organization admin information
6. **`w_organization_social`** - Permission to post content on behalf of organizations (company pages)

## Additional References

### Settings Page Documentation

**File:** `app/settings/page.tsx`  
**Line:** 542

The settings page shows a simplified list of required scopes:

```typescript
<p className="mt-3 text-blue-700"><strong>Required scopes:</strong> openid, profile, email, w_member_social</p>
```

Note: This documentation shows the basic scopes but doesn't include the organization scopes (`r_organization_admin`, `w_organization_social`).

## OAuth Flow Files

### 1. Connect Route
- **File:** `app/api/auth/connect/linkedin/route.ts`
- **Purpose:** Initiates the LinkedIn OAuth flow with the defined scopes
- **Scope Definition:** Line 26

### 2. Callback Route
- **File:** `app/api/auth/callback/linkedin/route.ts`
- **Purpose:** Handles the OAuth callback and exchanges the authorization code for access tokens
- **Scope Usage:** Uses the token granted with the scopes defined in the connect route

### 3. LinkedIn API Functions
- **File:** `lib/social/linkedin.ts`
- **Purpose:** Contains helper functions for LinkedIn API operations
- **Note:** This file doesn't define scopes but uses the access tokens that were granted with the defined scopes

## Environment Variables

LinkedIn credentials are configured through environment variables:

**File:** `.env.example`  
**Lines:** 23-25

```
# LinkedIn
LINKEDIN_CLIENT_ID=xxxxx
LINKEDIN_CLIENT_SECRET=xxxxx
```

## Organization Support

The application includes support for LinkedIn Company Pages (organizations):

- When a user authenticates, the callback route attempts to fetch organizations they manage
- If multiple organizations are found, the user is redirected to select which one to connect
- This functionality requires the `r_organization_admin` and `w_organization_social` scopes
- If these scopes are not approved by LinkedIn, the application falls back to personal profile connection

See `app/api/auth/callback/linkedin/route.ts` lines 13-61 for organization fetching logic.

## Summary

The LinkedIn scopes are **primarily defined in one location**: `app/api/auth/connect/linkedin/route.ts` at line 26. This is the authoritative source for which permissions the application requests from LinkedIn during the OAuth flow.
