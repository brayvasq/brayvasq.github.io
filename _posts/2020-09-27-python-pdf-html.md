---
layout: post
title:  "Create PDFs using Python and xhtml2pdf"
date:   2020-09-27 17:30:00 -0500
categories: tutorial python
tags: python api requests
---
Hi Everyone!. In this post, I want to share with you a little guide that will show you how to create pdf files using Python and [xhtml2pdf](https://github.com/xhtml2pdf/xhtml2pdf).

The [xhtml2pdf](https://github.com/xhtml2pdf/xhtml2pdf) lib is used to create pdf files from HTML files. It's just a guide that I made for myself, but I want to share it with you. 

**WARNING:** Also, this is a guide I did with experimental purposes, I didn't use it in a production environment. Therefore, you must have in mind that its use in production could have issues. A better alternative to create PDF files is http://weasyprint.org/.

## Requirements

- [Python](https://www.python.org/) (I use Python 3.8.2)
- Pip (I use Pip 20.1.1)

## Setup Project

To create our project we are going to use [virtualenv](https://pypi.org/project/virtualenv/), to create an isolated python environment for this project. However, you can also use [pyenv](https://github.com/pyenv/pyenv) or [venv](https://docs.python.org/3/library/venv.html).

So, first, we have to install `venv`.

```bash
pip3 install virtualenv
```

Now, we have to create the project folder and set up the virtualenv.

```bash
# Creating a project folder
mkdir pdfs-example
cd pdfs-example

# Creating the virtual environment
virtualenv env

# Activate the virtual environment
source env/bin/activate

# Create our main file
touch main.py
```

**NOTE:** To exit the environment you just have to write `deactivate`.

## Install dependencies

To create our PDF files we need to install the `xhtml2pdf` library. This library, also depends on `html5lib` and  `reportlab`.

```bash
pip install reportlab # https://pypi.org/project/reportlab/
pip install html5lib # https://pypi.org/project/html5lib/
pip install xhtml2pdf
```

**NOTE:** We need an xhtml2pdf version higher than `0.1a1` to work with Python3.

We can see the installed dependencies with the following command.

```bash
# Installed dependencies
pip freeze

# The above mentioned command will list something like the following
html5lib==1.1
Pillow==7.2.0
PyPDF2==1.26.0
reportlab==3.5.50
six==1.15.0
webencodings==0.5.1
xhtml2pdf==0.2.4
```

We can also export our dependencies.

```bash
pip freeze > requirements.txt
```

And install our dependencies from a `requirements.txt` file.

```bash
pip install -r requirements.txt
```

## Generate PDF from string

Now that we have the necessary modules installed, we can start writing code. First, we must import the [xhtml2pdf](https://github.com/xhtml2pdf/xhtml2pdf) module, which will help us to create our PDF files.

```python
# main.py
# import section ....
from xhtml2pdf import pisa             # import python module
# ....
```

Now, we can define some constants.

```python
# main.py

# Constants section ....
# Content to write in our PDF file.
SOURCE = "<html><body><p>PDF from string</p></body></html>"

# Filename for our PDF file.
OUTPUT_FILENAME = "test.pdf"
# ....
```

Ok, We will create a base function to reuse in the other functions and avoid code duplication.

```python
# main.py

# Methods section ....
def html_to_pdf(content, output):
    """
    Generate a pdf using a string content
    
    Parameters
    ----------
    content : str
	    content to write in the pdf file
	output  : str
	    name of the file to create
    """
    # Open file to write
    result_file = open(output, "w+b") # w+b to write in binary mode.
    
    # convert HTML to PDF
    pisa_status = pisa.CreatePDF(
            content,                   # the HTML to convert
            dest=result_file	       # file handle to recieve result
    )           

    # close output file
    result_file.close()

    result = pisa_status.err

    if not result:
        print("Successfully created PDF")
    else:
        print("Error: unable to create the PDF")    

    # return False on success and True on errors
    return result

# ....
```

Once we have the base function we can create our `from_text` function.

```python
# main.py

# Methods section ....
def from_text(source, output):
    """
    Generate a pdf from a plain string
    
    Parameters
    ----------
    source : str
	    content to write in the pdf file
	output  : str
	    name of the file to create
    """
    html_to_pdf(source, output)

# ....
```

Our `main` function will be the following.

```python
# main.py
# import section ....
import sys

# Main section ...
if __name__ == "__main__":
    if len(sys.argv)> 1 :
        if sys.argv[1] == '--help':
            print('Info: ')
            print('--help List the options to send an email')
            print('--text Create a PDF file from a string')
            print('--template Create a PDF file from a template')
        elif sys.argv[1] == '--text':
            print("Creating a PDF file from a string")
            from_text(SOURCE, OUTPUT_FILENAME)
    else:
        print("Please give the type of message to send.")
        print("For help execute `python main.py --help`")
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --text

# Creating a PDF file from a string
# Successfully created PDF
```

## Generate PDF from template

Here, we will generate a PDF file using an HTML template. We have to keep in mind that [xhtml2pdf](https://github.com/xhtml2pdf/xhtml2pdf) supports until HTML4. So, first, we have to create an HTML file that will behave as a template for our PDF file.

```bash
touch template.html
```

And we will define a simple html template.

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>PDF Generator</title>
</head>
<body>
	<h1 style="color:red;">First PDF</h1>
	<h2 style="color:blue;">PDF with html template</h2>
	<p>John</p>
	<p>Snow</p>
	<p>35</p>
</body>
</html>
```

Create a new constant to define our template file.

```python
# main.py

# Constants section ....
# Template file name
TEMPLATE_FILE = "template.html"
# ....
```

Now, We can create our function to read the template and create the PDF file.

```python
# main.py

# Methods section ....
def from_template(template, output):
    """
    Generate a pdf from a html file
    
    Parameters
    ----------
    source : str
	    content to write in the pdf file
	output  : str
	    name of the file to create
    """
    # Reading our template
    source_html = open(template, "r")
    content = source_html.read() # the HTML to convert
    source_html.close() # close template file
    
    html_to_pdf(content, output)

# ....
```

Add the option to our main function.

```python
# main.py

# Main section ...
if __name__ == "__main__":
	# ....
    
    if len(sys.argv)> 1 :
        # if ....
        elif sys.argv[1] == '--template':
            print("Creating a PDF file from a template")
            from_template(TEMPLATE_FILE, OUTPUT_FILENAME)
    else:
        # ....
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --template

# Creating a PDF file from a template
# Successfully created PDF
```

## Final Words

Thanks for reading this post and you can find the code of this guide [here](https://gist.github.com/brayvasq/02fb7616bdf8a19722268eba8c593d6c).

