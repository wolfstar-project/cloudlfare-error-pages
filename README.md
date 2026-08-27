# WolfStar error pages

Static, self-contained error pages styled with the WolfStar design system and ready to be fetched as [Cloudflare Custom Error assets](https://developers.cloudflare.com/rules/custom-errors/).

| Directory      | Intended response         | Cloudflare setup                                               |
| -------------- | ------------------------- | -------------------------------------------------------------- |
| `unauthorized` | `401 Unauthorized`        | Custom Error Rule                                              |
| `forbidden`    | `403 Forbidden`           | Custom Error Rule or block page                                |
| `not_found`    | `404 Not Found`           | Custom Error Rule                                              |
| `direct_ip`    | `421 Misdirected Request` | Custom Error Rule matching direct-IP traffic                   |
| `borked`       | `5XX` server errors       | 500 class Error Page; includes `::CLOUDFLARE_ERROR_500S_BOX::` |

## Cloudflare usage

1. Publish the required directory so its `index.html` is reachable over HTTPS and returns `200 OK` while Cloudflare fetches it.
2. In **Rules → Custom Errors**, add the page URL as a custom error asset or select it for the corresponding Error Page type.
3. Create a Custom Error Rule for the desired status or request condition, select the asset, and set the response status explicitly.
4. When the source changes, use **Fetch custom page again** in Cloudflare.

Each file has a complete `<head>`, contains no `referrer` meta tag or external dependency, and is far below Cloudflare's 1.5 MB processed-page limit. The pages intentionally use fixed HTTP copy instead of server-side Caddy template placeholders so Cloudflare can store and serve them verbatim. Their footer includes Cloudflare's `::RAY_ID::` token for request-level diagnostics.

For `borked`, keep the Cloudflare token unchanged. It is replaced with the diagnostic content when the 500 class Error Page is served. Cloudflare Error Pages do not cover status `500`, `501`, `503`, or `505`; use a Custom Error Rule if those exact origin responses also need this design.

## Formatting

Install the development dependencies and use the project-local Oxfmt commands:

```sh
npm install
npm run format
npm run format:check
```

## License

These error pages are licensed under the GPL-3.0 license. You can find the full text of the license in the `LICENSE` file.
