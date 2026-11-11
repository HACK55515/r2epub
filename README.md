# ReSpec to EPUB

Convert [ReSpec](https://respec.org/docs/) specifications and collections of HTML documents into **EPUB 3.4** publications.

```text
HTML / ReSpec source  →  r2epub  →  EPUB 3.4
```

r2epub can process an already-generated ReSpec document, ask the W3C Spec Generator to prepare a ReSpec source on the fly, or combine multiple source documents into one EPUB.

## What you can do

- **Convert one document** — create an EPUB from a ReSpec-generated HTML file.
- **Pre-process ReSpec sources** — generate the final ReSpec output as part of the conversion.
- **Build collections** — combine multiple HTML sources into a single EPUB 3 publication.
- **Run it where you need it** — use the command-line tool, HTTP service, or TypeScript/JavaScript API.

> **Service dependency:** on-the-fly ReSpec processing uses the [W3C Spec Generator](https://github.com/w3c/spec-generator). That external service can be slow or temporarily unavailable; r2epub cannot control its availability.

## Try it

### Convert a document from the command line

No local installation is required when using Deno:

```sh
deno run -A jsr:@iherman/r2epub/cli [filename]
```

Replace `[filename]` with the path or URL of the source document.

### Start the HTTP service

```sh
deno run -A jsr:@iherman/r2epub/serve
```

The service accepts conversion requests. See the [HTTP service guide](https://iherman.github.io/r2epub/doc/serve/) for endpoints and request examples.

### Use a local clone

From the repository directory:

```sh
# Convert a document
deno run -A r2epub.ts [filename]

# Start the service
deno run -A serve.ts

# Build standalone executables
deno task compile
```

The compile task creates the `r2epub` and `serve` executables.

## Use the hosted service

You can use either hosted deployment:

- **W3C service:** [labs.w3.org/r2epub](https://labs.w3.org/r2epub)
- **Deno Deploy:** [r2epub.iherman.deno.net](https://r2epub.iherman.deno.net)

A browser-based interface is also available at [Convert ReSpec to EPUB](https://iherman.github.io/r2epub/convert.html). The W3C service is integrated with ReSpec's export workflow.

## Use the TypeScript or JavaScript API

The package is available through [JSR](https://jsr.io/@iherman/r2epub). A minimal Deno example:

```ts
import * as r2epub from "jsr:@iherman/r2epub";

// Conversion is asynchronous because the source may need to be fetched.
const sourceUrl = "https://www.example.org/doc.html";
const options: r2epub.Options = {
  respec: false,
  config: {},
};

const epub = await r2epub.convert(sourceUrl, options);
const content = await epub.get_content();

// `content` contains the finalized EPUB data.
// Write it to a file using the file APIs for your runtime.
```

For the complete API, options, and examples, see the [API documentation](https://iherman.github.io/r2epub/doc/).

## Documentation

| Guide | What it covers |
| --- | --- |
| [Command-line guide](https://iherman.github.io/r2epub/doc/cli/) | CLI options and conversion examples |
| [HTTP service guide](https://iherman.github.io/r2epub/doc/serve/) | Running the server and making requests |
| [API reference](https://iherman.github.io/r2epub/doc/) | TypeScript/JavaScript API |
| [Browser converter](https://iherman.github.io/r2epub/convert.html) | Web interface for conversion |

## Configuration

The service recognizes the following environment variables:

| Variable | Purpose |
| --- | --- |
| `PORT` | Server port. Takes precedence over `R2EPUB_PORT`. |
| `R2EPUB_PORT` | Alternative server port; defaults to `80` when neither port variable is set. |
| `R2EPUB_LOCAL` | Allows localhost URLs for local testing. Set this only in a trusted development environment; do not enable it on a public deployment. |
| `R2EPUB_MODIFIED_EPUB_FILES` | Uses a local directory as the source for the W3C-specific assets adapted for EPUB. The default asset base is `https://www.w3.org/People/Ivan/r2epub/`. |

A local copy of the required distribution assets is included in the repository and can be used for local testing. This can also avoid bursts of requests to the remote asset host.

## About

r2epub is a TypeScript tool for turning W3C ReSpec material into EPUB 3.4. It is maintained by [Ivan Herman](https://www.ivan-herman.net) ([@iherman](https://github.com/iherman)).

Copyright © 2025 Ivan Herman.

---

If you find a problem or have an idea for an improvement, please open an issue or contribute a pull request.
