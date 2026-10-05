---
layout: page
permalink: /publications/
title: Publications
nav: true
nav_order: 3
publication_urls:
  golestaneh2026aura: "https://arxiv.org/abs/2605.27699"
  golestaneh2026metapusher: "https://arxiv.org/abs/2609.21122"
  zhong2026kite: "https://arxiv.org/abs/2605.09046"
  shiyas2026coad: "https://arxiv.org/abs/2603.12488"
  zhong2026activepusher: "https://arxiv.org/abs/2506.04646"
  zhong2024expansiongrr: "https://ieeexplore.ieee.org/document/10801917"
  boguslavskii2023nursingrobot: "https://ieeexplore.ieee.org/document/10342401"
  hu2023selfsupervised: "https://ieeexplore.ieee.org/document/10415669"
---

<style>
.publications ol.bibliography > li > .row {
	display: grid;
	grid-template-columns: 250px minmax(0, 1fr);
	gap: 1.1rem;
	margin: 0 0 1rem;
}

.publications ol.bibliography > li > .row > [class*="col-"] {
	width: auto;
	max-width: none;
	padding: 0;
}

.publications ol.bibliography > li > .row > .abbr {
	grid-column: 1;
}

.publications ol.bibliography > li > .row > :not(.abbr) {
	grid-column: 2;
}

.publications ol.bibliography .preview {
	width: 100%;
	max-width: 250px;
	height: auto;
}

.publications .publication-primary-link {
	color: inherit;
	text-decoration: none;
}

.publications .publication-primary-link:hover {
	color: var(--global-theme-color);
	text-decoration: underline;
}

.publications .publication-preview-link {
	display: inline-block;
}

@media (max-width: 767.98px) {
	.publications ol.bibliography > li > .row {
		grid-template-columns: minmax(0, 1fr);
	}

	.publications ol.bibliography > li > .row > .abbr,
	.publications ol.bibliography > li > .row > :not(.abbr) {
		grid-column: 1;
	}

	.publications ol.bibliography .preview {
		max-width: 360px;
	}
}
</style>

<div class="publications">

{% bibliography %}

</div>

<script>
(() => {
	const publicationUrls = {{ page.publication_urls | jsonify }};
	const buttonOrder = ["Website", "URL", "arXiv", "PDF", "Video", "Code", "Abs", "Bib", "DOI"];
	const primaryLinkOrder = ["Website", "Video", "URL", "PDF"];

	document.querySelectorAll(".publications ol.bibliography > li").forEach((item) => {
		const key = item.querySelector("[id]")?.id;
		const links = item.querySelector(".links");
		const url = publicationUrls[key];

		if (!links) return;

		if (url && !Array.from(links.querySelectorAll("a")).some((link) => link.textContent.trim() === "URL")) {
			const urlLink = document.createElement("a");
			urlLink.href = url;
			urlLink.className = "btn btn-sm z-depth-0";
			urlLink.setAttribute("role", "button");
			urlLink.target = "_blank";
			urlLink.rel = "noopener noreferrer";
			urlLink.textContent = "URL";
			links.append(urlLink);
		}

		const actions = Array.from(links.querySelectorAll("a"));
		for (const label of buttonOrder) {
			const action = actions.find((link) => link.textContent.trim() === label);
			if (action) links.append(action);
		}

		const primaryLink = primaryLinkOrder
			.map((label) => Array.from(links.querySelectorAll("a")).find((link) => link.textContent.trim() === label))
			.find(Boolean);
		if (!primaryLink) return;

		const title = item.querySelector(".title");
		if (title && !title.querySelector("a")) {
			const titleLink = document.createElement("a");
			titleLink.className = "publication-primary-link";
			titleLink.href = primaryLink.href;
			titleLink.target = "_blank";
			titleLink.rel = "noopener noreferrer";
			titleLink.textContent = title.textContent.trim();
			title.replaceChildren(titleLink);
		}

		const preview = item.querySelector("img.preview");
		if (preview && !preview.closest("a")) {
			preview.classList.remove("medium-zoom-image");
			preview.removeAttribute("data-zoomable");
			preview.alt = title?.textContent.trim() || preview.alt;
			const media = preview.closest("picture") || preview;
			const previewLink = document.createElement("a");
			previewLink.className = "publication-preview-link";
			previewLink.href = primaryLink.href;
			previewLink.target = "_blank";
			previewLink.rel = "noopener noreferrer";
			media.parentNode.insertBefore(previewLink, media);
			previewLink.append(media);
		}
	});
})();
</script>
