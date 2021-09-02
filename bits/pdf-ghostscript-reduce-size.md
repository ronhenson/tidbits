---
title: "Pdf Ghostscript Reduce Size"
created: 2021-08-11 17:37:52
tags: #tidbit
keywords: gs, ghostscript, pdf
---

# [Pdf Ghostscript Reduce Size](https://itsfoss.com/compress-pdf-linux/)

## **Note**: dPDFSETTINGS=/prepress

| dPDFSETTINGS | Description |
| :----------- | :---------- |
| /prepress (default) |	Higher quality output (300 dpi) but bigger size |
| /ebook | Medium quality output (150 dpi) with moderate output file size |
| /screen | Lower quality output (72 dpi) but smallest possible output file size | 

```bash
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/prepress -dNOPAUSE -dQUIET -dBATCH -sOutputFile=compressed_PDF_file.pdf input_PDF_file.pdf
```
