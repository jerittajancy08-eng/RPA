## Html:

Html tags represent browser tabs.

a) very reliable: app, url
b) alright: title, htmlwindowname, mobiledata

CRITICAL: Do not change `app` or `mobiledata` attributes if present on HTML selectors — critical to identify correct browser technology.

For browser tabs, prefer `url` over `title` when differentiating tabs — URLs (especially domains) far more stable than page titles. Wildcard URL paths (e.g., `url='https://example.com/*'`) to absorb within-site navigation while keeping domain anchor.

Before wildcarding html attributes, scan Window Level Context for other elements that would also match — if wildcarding collides with other open tabs, use more specific attribute or tighter wildcard.