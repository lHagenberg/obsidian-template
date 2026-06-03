<%*
// ─── REFRESH ANNOTATIONS & HIGHLIGHTS ────────────────────────────────────────
// Calls z.runImport() directly with the citekey from frontmatter.
// Uses the vault adapter to safely manage the hidden .temp folder.
// ─────────────────────────────────────────────────────────────────────────────

const SECTION_HEADER = "# Annotations & Highlights";
const IMPORT_FORMAT  = "annotations-only"; // must match name in Zotero Integration settings
const TEMP_FOLDER    = ".temp";            // hidden from Obsidian file explorer
const LIBRARY_ID     = 1;

// ── 1. Get citekey ────────────────────────────────────────────────────────────
const citekey = tp.frontmatter?.citekey;
if (!citekey) {
  new Notice("⚠️ No citekey found in frontmatter. Aborting.");
  return;
}

// ── 2. Get plugin ─────────────────────────────────────────────────────────────
const z = app.plugins.plugins["obsidian-zotero-desktop-connector"];
if (!z) {
  new Notice("⚠️ Zotero Desktop Connector plugin not found.");
  return;
}

// ── 3. Ensure .tmp folder exists and pre-delete any existing temp file ────────
// Use adapter directly so dot-folders are handled correctly
const adapter = app.vault.adapter;
const tempFilePath = `${TEMP_FOLDER}/${citekey}.md`;

const folderExists = await adapter.exists(TEMP_FOLDER);
if (!folderExists) {
  await adapter.mkdir(TEMP_FOLDER);
}

const fileExists = await adapter.exists(tempFilePath);
if (fileExists) {
  await adapter.remove(tempFilePath);
}

// ── 4. Run the import ─────────────────────────────────────────────────────────
try {
  await z.runImport(IMPORT_FORMAT, citekey, LIBRARY_ID);
} catch (e) {
  new Notice("⚠️ runImport failed: " + e.message);
  return;
}

// ── 5. Read the rendered output via adapter ───────────────────────────────────
const outputExists = await adapter.exists(tempFilePath);
if (!outputExists) {
  new Notice("⚠️ Import ran but output file not found. Check the output path in format settings.");
  return;
}

const rendered = await adapter.read(tempFilePath);
await adapter.remove(tempFilePath);

if (!rendered.trim()) {
  new Notice("⚠️ Zotero returned no content. Is Zotero open with the paper loaded?");
  return;
}

// ── 6. Splice into the real note ──────────────────────────────────────────────
const realFile = tp.file.find_tfile(tp.file.title);
const noteContent = await app.vault.read(realFile);

const sectionIndex = noteContent.indexOf(SECTION_HEADER);
const newBlock = SECTION_HEADER + "\n" + rendered.trim() + "\n";

let newContent;
if (sectionIndex !== -1) {
  newContent = noteContent.slice(0, sectionIndex).trimEnd() + "\n\n" + newBlock;
} else {
  newContent = noteContent.trimEnd() + "\n\n" + newBlock;
  new Notice("ℹ️ Section not found — appended at end of note.");
}

await app.vault.modify(realFile, newContent);

const count = (rendered.match(/^- > /gm) || []).length;
new Notice(`✅ Annotations refreshed — ${count} highlight${count !== 1 ? "s" : ""}.`);
-%>
