# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: masthead.spec.ts >> Masthead - Logo verification
- Location: specs/masthead.spec.ts:8:1

# Error details

```
Error: expect(locator).toHaveAttribute(expected) failed

Locator:  locator('.pf-v6-c-brand')
Expected: "/red-hat-logo-default-transparent-100px.png"
Received: "/red-hat-logo-reverse-transparent-100px.png"
Timeout:  5000ms

Call log:
  - Expect "toHaveAttribute" with timeout 5000ms
  - waiting for locator('.pf-v6-c-brand')
    9 × locator resolved to <img class="pf-v6-c-brand" alt="Red Hat build of Apicurio Registry" src="/red-hat-logo-reverse-transparent-100px.png"/>
      - unexpected value "/red-hat-logo-reverse-transparent-100px.png"

```

# Page snapshot

```yaml
- generic [ref=e3]:
  - banner [ref=e4]:
    - link "Red Hat build of Apicurio Registry" [ref=e7] [cursor=pointer]:
      - /url: /explore
      - img "Red Hat build of Apicurio Registry" [ref=e8]
    - generic [ref=e12]:
      - generic [ref=e16]: Red Hat build of Apicurio Registry
      - button [ref=e19] [cursor=pointer]:
        - img [ref=e21]
  - main [ref=e24]:
    - tablist [ref=e28]:
      - tab "Dashboard" [selected] [ref=e29] [cursor=pointer]:
        - generic [ref=e30]: Dashboard
      - tab "Explore" [ref=e31] [cursor=pointer]:
        - generic [ref=e32]: Explore
      - tab "Search" [ref=e33] [cursor=pointer]:
        - generic [ref=e34]: Search
      - tab "Global rules" [ref=e35] [cursor=pointer]:
        - generic [ref=e36]: Global rules
      - tab "Settings" [ref=e37] [cursor=pointer]:
        - generic [ref=e38]: Settings
    - generic [ref=e40]:
      - heading "Registry Dashboard" [level=1] [ref=e42]
      - paragraph [ref=e44]: Welcome to Apicurio Registry. Manage your schemas and API definitions in one central location.
    - generic [ref=e46]:
      - generic [ref=e48]:
        - generic [ref=e50] [cursor=pointer]:
          - generic [ref=e52]: Groups
          - generic [ref=e54]:
            - img [ref=e56]
            - generic [ref=e58]: "1"
        - generic [ref=e60] [cursor=pointer]:
          - generic [ref=e62]: Artifacts
          - generic [ref=e64]:
            - img [ref=e66]
            - generic [ref=e68]: "1"
        - generic [ref=e70] [cursor=pointer]:
          - generic [ref=e72]: Versions
          - generic [ref=e74]:
            - img [ref=e76]
            - generic [ref=e78]: "2"
      - generic [ref=e80]:
        - generic [ref=e82]: Recent Artifacts
        - generic [ref=e86]:
          - generic "OpenAPI Definition" [ref=e88]
          - generic [ref=e89]:
            - link "Empty API Spec" [ref=e90] [cursor=pointer]:
              - /url: /explore/ExploreTest/MyArtifact
            - generic [ref=e91]: ExploreTest/MyArtifact
          - generic [ref=e93]: 42 seconds ago
      - generic [ref=e95]:
        - generic [ref=e97]: Quick Actions
        - generic [ref=e99]:
          - button "Create Artifact" [ref=e101] [cursor=pointer]:
            - img [ref=e103]
            - generic [ref=e105]: Create Artifact
          - button "Search Registry" [ref=e107] [cursor=pointer]:
            - img [ref=e109]
            - generic [ref=e111]: Search Registry
          - button "Explore Groups" [ref=e113] [cursor=pointer]:
            - img [ref=e115]
            - generic [ref=e117]: Explore Groups
          - button "Settings" [ref=e119] [cursor=pointer]:
            - img [ref=e121]
            - generic [ref=e123]: Settings
```

# Test source

```ts
  1  | import { test, expect } from "@playwright/test";
  2  | 
  3  | const REGISTRY_UI_URL: string = process.env["REGISTRY_UI_URL"] || "http://localhost:8888/";
  4  | const IS_DOWNSTREAM: boolean = process.env["IS_DOWNSTREAM"] === "true";
  5  | const EXPECTED_ALT: string = process.env["EXPECTED_ALT"] || IS_DOWNSTREAM ? "Red Hat build of Apicurio Registry" : "Apicurio Registry";
  6  | const EXPECTED_LOGO_SRC: string = process.env["EXPECTED_LOGO_SRC"] || IS_DOWNSTREAM ? "/red-hat-logo-default-transparent-100px.png" : "/apicurio_registry_logo_default.svg";
  7  | 
  8  | test("Masthead - Logo verification", async ({ page }) => {
  9  |     await page.goto(REGISTRY_UI_URL);
  10 | 
  11 |     // Wait for the page to load by checking the title
  12 |     await expect(page).toHaveTitle(/Apicurio Registry/);
  13 | 
  14 |     // Locate the masthead logo image
  15 |     const logoImage = page.locator(".pf-v6-c-brand");
  16 | 
  17 |     // Wait for the logo image to be visible
  18 |     await expect(logoImage).toBeVisible();
  19 | 
  20 |     // Verify the src attribute
> 21 |     await expect(logoImage).toHaveAttribute("src", EXPECTED_LOGO_SRC);
     |                             ^ Error: expect(locator).toHaveAttribute(expected) failed
  22 | 
  23 |     // Verify the alt text
  24 |     await expect(logoImage).toHaveAttribute("alt", EXPECTED_ALT);
  25 | });
  26 | 
```