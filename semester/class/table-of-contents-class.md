```dataviewjs
// Get all files in the vault
const folder = "semester/class/lecture-notes";
const pdfFiles = app.vault.getFiles()
  .filter(file => file.path.startsWith(folder) && file.extension === "pdf")
  .sort((a, b) => b.name.localeCompare(a.name)); // Sort in reverse alphabetical order

if (pdfFiles.length > 0) {
  const table = dv.table(["Lecture Notes"],
    pdfFiles.map(file => [
      dv.fileLink(file.path),
    ])
  );
} else {
  dv.paragraph("No PDF files found in " + folder);
}
```
