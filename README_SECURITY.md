# Managed Firewall Dashboard – publishing safely

**What is in the repo (safe to publish)**
- `Managed_Firewall_Dashboard.html` – no username, password, hash or salt inside.
- `Credential_Setup.html` – generator; contains no secrets.
- `auth.config.example.js`, `.gitignore`, this file.

**What stays private (never commit)**
- `auth.config.js` – salted hash of the NOC credentials. It is git-ignored.

**Setup**
1. Open `Credential_Setup.html`, enter a username and a long password (14+ characters is better), download `auth.config.js`.
2. Put `auth.config.js` in the same folder as `Managed_Firewall_Dashboard.html`.
3. Without that file the login screen refuses all sign-ins ("Sign-in is not configured").

**Limits you should know**
- This is a client-side gate. Anyone who can load the dashboard *and* `auth.config.js` can read the hash and try to guess the password offline. Do not host both on a public URL (for example GitHub Pages). Keep `auth.config.js` on an internal share / private host.
- For real access control on a public host, put the site behind server-side authentication (Cloudflare Access, Azure AD / SharePoint permissions, a reverse proxy).
- The password was shared in chat while this was built: generate a new one with the setup page and retire the old one.
- Before pushing, run: `git ls-files | grep -i auth.config` (should list only `auth.config.example.js`) and `git log -p | grep -i -E "salt|hash:"`.
