# Deployment instructions

## Existing target

Configuration recorded 7 November 2026; re-check the live project before deployment.

- Netlify team: `wwwdycg`; project: `aws-sap-mock`.
- Site ID: `c870e34c-8eaf-429f-9164-12361c4eb1c1`.
- Primary URL: https://aws-sap.m1ngshum.com
- Existing Netlify hostname: `aws-sap-mock.netlify.app`.
- Dashboard: https://app.netlify.com/projects/aws-sap-mock

## Build and publication

- GitHub: `m1ngshum/aws-sap-c02-simulator`; the existing Netlify project is linked to `main`.
- Static HTML, CSS, JavaScript, and question JSON; no package install or build is required. Netlify's build command and publish directory are currently unset; inspect the existing configuration before changing it.
- Local preview: `python3 -m http.server 8080 --bind 127.0.0.1`; check question loading, navigation, and results without overwriting existing saved progress.
- Legacy `.github/workflows/deploy.yml` also targets Cloudflare Pages on pushes to `main`; `wrangler.toml` and the README retain that older path. Do not run Wrangler or assume Netlify team protection covers a Cloudflare Pages deployment. Check workflow status and any alternate exposure before pushing; changing/removing that workflow needs a separate scoped request.

Use the existing project; do not create a replacement or change production branches. A requested code change does not by itself authorize a manual production deployment. Pushing to a linked `main` can trigger production; report that effect before publishing changes. Keep unrelated work out of deployment commits.

## Access, DNS, and HTTPS

- Require **Netlify team login on all deployments**, including production, branch deploys, and deploy previews. Shared-password protection is disabled. These visitor controls live in Netlify project settings, not source code or `netlify.toml`; preserve them across deployments.
- Authorized Netlify team access is required. Do not make the site public, restore a shared password, invite people, configure Google OAuth, change team defaults, or upgrade plans as a deployment side effect.
- Cloudflare remains authoritative for DNS. The `aws-sap.m1ngshum.com` CNAME points to `aws-sap-mock.netlify.app`, DNS-only (`proxied=false`), TTL 300 seconds. Stop on conflicting records instead of replacing them. Preserve unrelated aliases.
- Use Netlify-managed HTTPS with HTTP-to-HTTPS redirects. Never bypass certificate validation to declare deployment success.
- Preserve the public `m1ngshum.com` homepage, DevOps dashboard, Cloudflare nameservers, MX/SPF/DKIM records, and `hello@m1ngshum.com` Email Routing. No inbox test is part of deployment verification.

## Acceptance after a requested deployment

1. Confirm the intended deploy is ready, with the expected source revision or staged files. Re-read visitor controls: team login scope **all**, password disabled.
2. Check anonymous HTTPS requests to the custom domain, existing `.netlify.app` URL, and the specific deploy/preview permalink. They must show Netlify's access gate (currently HTTP 401), never site content. Check relevant static assets and functions when changed.
3. Verify an authorized Netlify browser session loads real content at the custom domain. Anonymous checks alone are insufficient; report if a signed-in session is unavailable.
4. Verify the CNAME target, valid certificate, and HTTP redirect to HTTPS. Report DNS cache delays instead of weakening protection.
5. Check the site's affected routes/features. Report what was actually tested and any remaining limits; do not equate a ready deploy with application acceptance.
