<%*
const createdDate = String(tp.frontmatter["date"]);
const dateOnly = createdDate.split(" ")[0].replace(/-/g, "");
const participants = tp.frontmatter.participants;
const tags = tp.frontmatter["tags"] ?? [];

let prefix; 
if (tags.includes("type/meeting/agenda")) { 
    prefix = "a"; 
} else { 
    prefix = "m"; // default fallback 
} 

let suffix; 
if (tags.includes("type/meeting/clean")) {
    suffix = ".out"; 
} else { 
    suffix = ""; // default fallback 
} 

const newFileName = `${prefix}${dateOnly}(${participants})${suffix}`;
// rename the current note
await tp.file.rename(newFileName);
%> 