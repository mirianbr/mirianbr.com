+++
title = "A pytesseract hello world in Portuguese under Anaconda"
description = "Complementary tutorial for a hello world in OCR in Portuguese using (py)tesseract under Anaconda, with language-specific 'Failed loading language' error resolution"
type = "post"
tags = [
    "brazilian-portuguese",
    "pdf",
    "ocr",
    "tesseract",
    "pytesseract",
    "python",
    "data-scrapping",
    "anaconda",
    "tutorials"
]
date = "2021-07-13"
[ author ]
  name = "Mírian Bruckschen Motta Barros"
+++

Playing with OCR using python for the first time, I came across [tesseract](https://github.com/tesseract-ocr/tesseract) and its Python wrapper, [pytesseract](https://pypi.org/project/pytesseract/). I started by (and recommend) [this detailed tutorial](https://pythonforundergradengineers.com/how-to-install-pytesseract.html), that covers the tesseract and pytesseract installation, assuming you already have Anaconda.

## Forcing the tessdata-dir to be found

Depending on your installation, though, things may not work at first. Mine didn't, for the tessdata-dir wasn't being found (`Error opening data file...` error). In order to resolve this, I followed [pytesseract usage hint](https://pypi.org/project/pytesseract/) to force the tessdata directory:

1. Find the tessdata directory in my local installation
2. Create a [literal string variable](https://docs.python.org/3.3/reference/lexical_analysis.html?highlight=string%20literals#string-and-bytes-literals) (prefixed by `r'`) to store the directory path, so your backslashes are interpreted literally
3. Use this string when calling image_to_string in the `config` parameter

See my code snippet below:

```
tessdata_dir_config = r'--tessdata-dir "C:\Users\miria\anaconda3\envs\tesseract\Library\bin\tessdata"'
phrase = ocr.image_to_string(Image.open('oi.jpeg'), config=tessdata_dir_config)
```

## Failed loading language 'por', Tesseract couldn't load any languages!

If you want to use tesseract to read texts from images that are not in English (in my case, it's Portuguese), have in mind Anaconda may not have installed your target language. 

My English tutorials worked, but not other languages ([announced to be supported](https://github.com/tesseract-ocr/tesseract/blob/master/doc/tesseract.1.asc#languages)). Every time, I got the error below:

```
TesseractError: (1, 'Error opening data file <your tessdata dir>/por.traineddata 
  Please make sure the TESSDATA_PREFIX environment variable is set to your "tessdata" directory. 
  Failed loading language 'por' 
  Tesseract couldn't load any languages! 
  Could not initialize tesseract.')
```

The `couldn't load any languages` part led me to find the same answers as the previous section -- forcing the tessdata-dir configuration. Except it didn't happen for English. 

The relevant part of the error message was the `language 'por'`: tesseract didn't have Portuguese data. Understanding the error was much harder than the resolution:

1. Download your language data from the [tesseract repository](https://github.com/tesseract-ocr/tessdata/)
2. Save it in your tessdata directory

All done! 

Having the config and data set up, OCR with tesseract in Portuguese worked great. See how in [my Jupyter Notebook for this tutorial](https://github.com/mirianbr/vaccine-batches/blob/main/hello-ocr.ipynb). 😉
