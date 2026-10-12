# Sandbox notes for agents

This sandbox has an Xfce desktop (`DISPLAY=:1`) with LibreOffice (Writer, Calc, Impress) installed.

## Editing office documents: use UNO via `lo-uno`

Drive LibreOffice through its UNO API, not by clicking or by walking the accessibility tree.
`lo-uno` connects to the LibreOffice window on the desktop (starting it with a UNO socket if
needed), so edits show up live on screen.

```bash
lo-uno --help                                              # predefined names and examples
lo-uno -c 'd = new_doc("writer"); d.Text.setString("안녕하세요")'
lo-uno -c 'd = open_doc("/home/nightona/report.xlsx"); print(d.Sheets.getByIndex(0).getCellRangeByName("B2").getValue())'
lo-uno -c 'doc.store()'                                    # save the active document in place
lo-uno -c 'doc.storeToURL(to_url("/tmp/out.pdf"), props(FilterName="writer_pdf_Export"))'
lo-uno script.py                                           # longer scripts
```

- `lo-uno` runs on `/usr/bin/python3`. Do not `import uno` from plain `python3`
  (`/usr/local/bin/python3`); the module is not available there.
- For headless conversion with no desktop, `soffice --headless --convert-to pdf file.docx` also works,
  but not while the desktop LibreOffice is running with the same profile.
- Common export filters: `writer_pdf_Export`, `calc_pdf_Export`, `impress_pdf_Export`,
  `MS Word 2007 XML`, `Calc MS Excel 2007 XML`, `Impress MS PowerPoint 2007 XML`.

## Verifying

Take a screenshot to check the visual result after an edit. Read values back through UNO to
check content.

## Accessibility (AT-SPI)

`pyatspi` is available on `/usr/bin/python3` for inspecting what is on screen, such as which
dialog is open. Do **not** walk the full accessibility tree of a Calc document: it exposes every
cell and never finishes. Prefer UNO for anything inside a document.

## Fonts

Noto (most scripts, CJK, color emoji), Nanum (Korean), DejaVu, Liberation, Carlito and Caladea
(metric-compatible with Calibri and Cambria). Korean, Chinese, Japanese, Arabic, Hebrew, Indic,
Thai, Cyrillic and Greek text all render. List with `fc-list :lang=ko`.
