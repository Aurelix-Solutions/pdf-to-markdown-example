# PDF to Markdown Converter: Python example

**PDF Table Extraction - PDF to Markdown OCR API.** Turns linked PDF and Word files into Markdown per page, with tables as JSON rows, RAG chunks and OCR. Price, example input and output, and FAQ: [PDF to Markdown OCR & Table Extraction on the Aurelix tools site](https://aurelix-solutions.github.io/tools/pdf-to-markdown/). Run it on the Apify Store: [apify.com/aurelix/pdf-to-markdown](https://apify.com/aurelix/pdf-to-markdown).


A small, runnable example for [PDF to Markdown Converter](https://apify.com/aurelix/pdf-to-markdown), an Apify Actor by Aurelix.

[![PDF to Markdown OCR & Table Extraction: 75-second demo video](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/pdf-to-markdown-demo-thumb-v1.png)](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/pdf-to-markdown-demo-v1.mp4)

**[▶ Watch the 75-second demo](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/pdf-to-markdown-demo-v1.mp4)**

## What it does

PDF to Markdown Converter downloads the PDF or Word (DOCX) files you link and returns one row per page: the page as Markdown (headings from font sizes, paragraphs, bullet lists, numeric table rows kept one per line, ruled tables as Markdown tables), every table also as JSON rows, the page split into AI-ready chunks for embeddings, the word count and the document title. Pages without a text layer, such as scans, are read with the open-source Tesseract OCR engine in English, French, Spanish or German.

## Why this over alternatives

Compared with the leading alternatives on the Apify Store (the same table is on the Store page):

| | **This Actor** | memo23/pdf-text-extractor | automation-lab/pdf-text-extractor |
|---|---|---|---|
| **Price per 1,000, free plan** | $0.30 per 1,000 pages | $5 per 1,000 PDFs | $3.45 per 1,000 PDFs |
| **Price per 1,000, paid plans** | $0.25 per 1,000 pages | $5 per 1,000 PDFs | $3.00 per 1,000 PDFs |
| **Start fee per run** | $0.001 | $0.005 | $0.005 |
| **Platform usage billed on top** | No | No ($7 per 1,000 proxy MB if used) | No |
| **A 10-page PDF, free plan** | $0.003 | $0.005 | $0.00345 |
| **OCR for scanned pages** | ✓ $8 per 1,000 pages, automatic | ✓ $15 per 1,000 pages, opt-in | ✗ |
| **Markdown output** | ✓ per page | ✓ | ✗ |
| **Tables as JSON rows** | ✓ | not stated | not stated |
| **RAG chunks** | ✓ | not stated | ✗ |
| **DOCX (Word) files** | ✓ | ✗ | ✗ |
| **Uploaded files or base64** | ✗ (URLs) | ✓ | ✗ |

- $0.30 per 1,000 pages: $0.003 for a 10-page PDF, no per-file fee.
- Tables come back twice: as Markdown in the text and as JSON rows.
- Page-level chunks with overlap, ready for a vector database.
- Scanned pages read with OCR automatically at $8 per 1,000 OCR pages; text pages are never OCR'd.
- PDF and DOCX in one Actor; the file type is detected from the content.
- Failed downloads, non-documents and empty pages are never charged.

## When to use it

- AI developers loading reports, manuals and papers into a RAG pipeline
- Analysts pulling tables out of statistical releases and annual reports
- Teams digitising scanned letters and forms with OCR
- AI agents that need a document's text and tables through MCP

## Input example

This is the `pdf-to-markdown` input used in `main.py`:

```json
{
  "urls": [
    "https://arxiv.org/pdf/1706.03762"
  ]
}
```

Every input field is documented on the [Store page](https://apify.com/aurelix/pdf-to-markdown).

## How to run

1. Get an Apify API token from Apify Console (Settings, API & Integrations).
2. Install and run:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export APIFY_TOKEN=your_token_here
python main.py
```

The script starts a run of `aurelix/pdf-to-markdown`, prints a few fields from the first results and writes every result to `results.csv`. Lists and nested objects are stored in the CSV as JSON text. Each run is billed to your Apify account (see Pricing below).

## Sample output

One record as returned by a real run (long text is shortened):

```json
{
  "url": "https://calibre-ebook.com/downloads/demos/demo.docx",
  "file_name": "demo.docx",
  "file_type": "docx",
  "title": "DOCX Demo",
  "page": 1,
  "pages_total": 4,
  "markdown": "# Demonstration of DOCX support in calibre\n\nThis document demonstrates the ability of the calibre DOCX Input plugin to convert the various typographic features in a Microsoft Word (2007 and newer) document. Convert this document to a modern ebook format, such as AZW3 for Kindles or EPUB for other eb... [2634 more characters]",
  "tables": [
    {
      "index": 0,
      "rows": [
        [
          "ITEM",
          "NEEDED"
        ],
        [
          "Books",
          "1"
        ],
        [
          "Pens",
          "3"
        ],
        "... [3 more]"
      ]
    }
  ],
  "chunks": [
    "# Demonstration of DOCX support in calibre\n\nThis document demonstrates the ability of the calibre DOCX Input plugin to convert the various typographic features in a Microsoft Word (2007 and newer) document. Convert this document to a modern ebook format, such as AZW3 for Kindles or EPUB for other eb... [1571 more characters]",
    "formatting\n\nYou can do crazy things with paragraphs, if the urge strikes you. For instance this paragraph is right aligned and has a right border. It has also been given a light gray background.\n\nFor the lovers of poetry amongst you, paragraphs with hanging indents, like this often come in handy. Yo... [957 more characters]"
  ],
  "word_count": 513,
  "ocr": false,
  "source_url": "https://calibre-ebook.com/downloads/demos/demo.docx",
  "scraped_at": "2026-11-04T19:25:43Z"
}
```

## Export to CSV

`main.py` writes `results.csv` with the `csv` module. You can also download any run's results directly:

```
https://api.apify.com/v2/datasets/<datasetId>/items?format=csv&clean=true
```

Send your token in an `Authorization: Bearer <token>` header (or `?token=`) if the dataset is not readable without one. Other formats: `format=json`, `format=xlsx`.

## Google Sheets

- Simplest: use Apify's Google Sheets integration on the Actor or a saved task, so each run's results land in a sheet with no token in the sheet itself.
- Quick pull of one run: `=IMPORTDATA("https://api.apify.com/v2/datasets/<datasetId>/items?format=csv&clean=true")`. This only works without a token if your account lets anyone with the dataset ID read it. Do not paste an API token into a sheet formula: anyone who can view or copy the sheet can read it and use your account.

## n8n

- Apify node: choose "Run an Actor", enter `aurelix/pdf-to-markdown`, paste the input above, then read the run's dataset items.
- HTTP Request node: `POST https://api.apify.com/v2/acts/aurelix~pdf-to-markdown/run-sync-get-dataset-items` with header `Authorization: Bearer <token>` (stored as an n8n credential) and the input as the JSON body. The response is the list of results. This endpoint waits for the run to finish, so for large runs start the run asynchronously and fetch the dataset when it ends.

## Limits

- Multi-column layouts (papers, forms) are read line by line across the page, so columns can interleave.
- Tables without ruling lines are kept as text rows, not split into cells; merged cells come back as repeated or empty cells.
- OCR quality depends on the scan; handwriting is not supported, and OCR runs at about 3 to 10 seconds a page.
- Images, charts and formulas are not described; only their horizontal text labels come through (rotated or vertical text is skipped).
- Files over 50 MB, password-protected PDFs, legacy .doc and other formats are skipped without charge.
- Documents are read from the first page up to 'Max pages per document' (300 by default).
- A document stops after 5 blank scanned pages in a row (blank pages are never charged), so scans with long blank runs end early.

## Pricing

At the time of writing:

Pay per event: you pay only for what the run returns, plus a small start fee.

| Event | Free plan | Paid plans (Bronze, Silver, Gold) |
|---|---|---|
| `page-result` | $0.30 per 1,000 pages | $0.25 per 1,000 pages |
| `ocr-page-result` | $8.00 per 1,000 OCR pages | $5.60 per 1,000 OCR pages |
| `apify-actor-start` | $0.001 per run | $0.001 per run |

OCR pages are charged as ocr-page-result instead of page-result, never both. Not charged: URLs that fail to download, are blocked by robots.txt or are not a PDF or DOCX, and pages with no text even after OCR.

Check the [Store page](https://apify.com/aurelix/pdf-to-markdown) for current prices.

## Links

- Actor: https://apify.com/aurelix/pdf-to-markdown
- Questions, bugs and feature requests: the Issues tab on the Store page.

## Licence

The example code in this repository is released under the MIT licence (see `LICENSE`). The Actor itself runs on Apify and is billed through your Apify account.
