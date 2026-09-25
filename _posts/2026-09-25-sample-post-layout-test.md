---
# ---------------------------------------------------------------------------
# SAMPLE POST: shows every front matter field and every style.
# Delete this file before the blog goes live.
# ---------------------------------------------------------------------------

# Required. Used as the H1, the card title and the start of the browser/share
# title ("Title | Kemp & Kerrigan Opticians"). Keep it under about 60 characters.
title: "Sample post: layout test"

# Required. Meta description, share text and the excerpt on the card.
# One or two sentences, about 150 characters.
description: "Placeholder post used to check every part of the blog layout: headings, lists, quotes, images, FAQs and the call-to-action box."

# Required. Publication date and time (UK time). Posts dated in the future
# are not published until a build runs after that time.
date: 2026-09-25 09:00:00 +0100

# Optional. Set when a post is meaningfully updated. Shown as "Updated ..."
# and used as dateModified in structured data. Delete the line if unused.
last_modified_at: 2026-09-25 09:00:00 +0100

# Required. Hero image, also used on the card and when the post is shared.
# All four values are required; the build fails if alt is missing.
image:
  path: /assets/images/adam-kerrigan-consultation-desk.webp
  width: 1600
  height: 1067
  alt: "Adam Kerrigan at his desk in the practice, holding up a pair of glasses as he talks"

# Optional. Caption shown under the hero image.
image_caption: "Placeholder caption for the hero image."

# Optional. Frequently asked questions shown at the end of the post and
# output as FAQPage structured data. Delete the whole block if unused.
faqs:
  - question: "Sample question one: how does a short answer look?"
    answer: "Placeholder answer. This checks how a single short paragraph sits inside an open question."
  - question: "Sample question two: can an answer contain a link?"
    answer: "Placeholder answer. Answers can use Markdown, including [a link to the blog home](/). Internal links are written as /post-name/."
  - question: "Sample question three: what happens with a longer answer?"
    answer: |
      Placeholder answer, first paragraph. This one is longer so the spacing inside an answer can be checked across more than one line of text on a phone and on a desktop screen.

      Placeholder answer, second paragraph. It checks the gap between two paragraphs inside the same answer.

# Optional. Set to false to hide a post without deleting it.
published: true
---

Placeholder introduction. This post exists only to test the layout of the Kemp & Kerrigan blog, and none of this text should be read as information about the practice or about eye care. The opening paragraph sits directly under the hero image and should feel like the start of the article rather than a caption.

Placeholder paragraph. This second paragraph checks the gap between two paragraphs of body text. The line length should stay comfortable to read on a large screen, at no more than around sixty eight characters per line, and the text should fill the width of a phone with a twenty pixel margin either side.

## Sample question heading: how does an H2 sit with its answer?

Placeholder answer paragraph. A question heading like the one above should have noticeably more space above it than below it, so it reads as belonging to this paragraph rather than to the one before. The first sentence or two under each heading should answer the question directly.

Placeholder paragraph with [an example inline link](/) so link colour, underline and hover state can be checked against the surrounding text.

### Sample subheading at H3 level

Placeholder paragraph under a third level heading, to check the heading sizes step down clearly from H1 to H2 to H3.

Placeholder lead-in to a bulleted list:

- First placeholder list item, kept short.
- Second placeholder list item, which runs a little longer so it wraps onto a second line on a phone and shows how the wrapped line aligns.
- Third placeholder list item.

Placeholder paragraph after the list, to check the space between a list and the text that follows it.

> Placeholder quote. This checks the quote style: a tan rule on the left, slightly larger text, and extra space above and below.

Placeholder paragraph after the quote. The wide image below should break out beyond the text column on larger screens, while keeping the same rounded corners and the same caption style as the hero.

{% include figure.html src="/assets/images/adam-kerrigan-fitting-glasses.webp" alt="Adam Kerrigan checking how a pair of glasses sits on a young patient's face" caption="Placeholder caption for a wide image." class="wide" %}

## Sample question heading: what comes after an image?

Placeholder paragraph after the wide image. The gap between an image and the next paragraph should match the gap above the image.

Placeholder closing paragraph. After this come the frequently asked questions, then the call-to-action box, then "More from the blog", which stays hidden until a second post exists.
