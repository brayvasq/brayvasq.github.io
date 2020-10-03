---
layout: post
title:  "Send emails with Python and Gmail"
date:   2020-10-03 16:00:00 -0500
categories: tutorial python
tags: python emails gmail
---

Hi Everyone!. In this post, I want to share with you a little guide that will show you how to send emails using Python and Gmail.

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
mkdir emails-example
cd emails-example

# Creating the virtual environment
virtualenv env

# Activate the virtual environment
source env/bin/activate

# Create our main file
touch main.py
```

**NOTE:** To exit the environment you just have to write `deactivate`.

## Setup dotenv

Before starting to write code, we need to install the package `python-dotenv`, to manage environment variables, where we will put our Gmail credentials. We'll use `.env` files to easily configure the project and to avoid sharing sensitive info in our code.

```
pip install -U python-dotenv
```

Ok, now we should test if we can use the package. Create a file called `.env`.

```bash
touch .env
```

Create a test variable in our `.env` file.

```tex
TEST="my test variable"
```

Import the `dotenv` package and load the variables.

```python
# main.py
# Import section ....
import os
from dotenv import load_dotenv

# Loading ENV variables
load_dotenv()

# Obtain our TEST variable
print(os.getenv('TEST'))
```

Run the script.

```bash
python main.py
# my test variable
```
## Send basic emails

**NOTE:** Before start writing code, you have to allow external apps in your Gmail account.

First, we must import the python [smtplib](https://docs.python.org/3/library/smtplib.html) client, which will help us to send our emails.

````python
# main.py
# Import section ....
from smtplib import SMTP, SMTPException # Import SMTP module
# ....
````

We will create some environment variables that we need to configure our SMTP client.

```txt
# .env

SENDER="YOUR GMAIL EMAIL"
PASSWORD="YOUR GMAIL PASSWORD"
RECEIVERS="A LIST OF EMAILS TO SEND THE MESSAGE, SEPARATED USING ','"
HOST="smtp.gmail.com"
PORT="587"
```

Now, we can set some constants using the environment variables defined before.

```python
# main.py

# Constants section ....

# GMAIL account sender
SENDER = os.getenv('SENDER')
PASSWORD = os.getenv('PASSWORD')

# Accounts receivers
RECEIVERS = os.getenv('RECEIVERS').split(',')

# SMTP server options
HOST = os.getenv('HOST')
PORT = os.getenv('PORT')

# ....
# SMTP object
SMTP_OBJECT = SMTP(host=HOST, port=PORT)
```

Ok, We will create a base function to reuse in the other functions and avoid code duplication.

```python
# main.py

# Methods section ....

# Base function
def send_email(message=""):
    """
    Send an email using the credentials defined before
    
    Parameters
    ----------
    message : str
	    content to send in the message
    """
    try:
        SMTP_OBJECT.ehlo()
        SMTP_OBJECT.starttls()
        SMTP_OBJECT.login(SENDER, PASSWORD)
        SMTP_OBJECT.sendmail(SENDER, RECEIVERS, message)
        SMTP_OBJECT.quit()
        print("Successfully sent email")
    except SMTPException:
        print ("Error: unable to send email")
```

- [ehlo()](https://docs.python.org/3/library/smtplib.html#smtplib.SMTP.ehlo) Open a transmision using the command `EHLO`. Usually this method is called automatically when use sendmail() function. We also can use the function [ehlo_or_helo_if_needed()](https://docs.python.org/3/library/smtplib.html#smtplib.SMTP.ehlo_or_helo_if_needed) for this purpose.
- [starttls()](https://docs.python.org/3/library/smtplib.html#smtplib.SMTP.starttls) Uses the TLS protocol in the connection.
- [login()](https://docs.python.org/3/library/smtplib.html#smtplib.SMTP.login) Login on the SMTP server.
- [sendmail()](https://docs.python.org/3/library/smtplib.html#smtplib.SMTP.sendmail) Sends our email.
- [quit()](https://docs.python.org/3/library/smtplib.html#smtplib.SMTP.quit) Terminate the session and close the connection.

Once we have the base function we can create our `basic_mail` function.

```python
# main.py
# Methods section ....

def basic_email(message = ""):
    """
    Basic email with plain text message
    """
    send_email(message)
```

Our `main` function will be the following.

```python
# main.py
# Import section ....
import sys

# Main section ...
if __name__ == "__main__":
    # Message to send
    message = """
    this message is sent from python for testing purposes
    """
    
    if len(sys.argv)> 1 :
        if sys.argv[1] == '--help':
            print('Info: ')
            print('--help List the options to send an email')
            print('--basic Sends a plain text email')
            print('--txt Send an email from a text file content')
            print('--attach Send an email with an attachment')
        elif sys.argv[1] == '--basic':
            print("Sending a plain text email")
            basic_email(message)
    else:
        print("Please give the type of message to send.")
        print("For help execute `python main.py --help`")
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --basic

# Sending a plain text email
# Successfully sent email
```

## Send email from TXT file

Here, we will send a message from a `.txt` file. First, we have to import some useful functions.

```python
# main.py
# Import section ....
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.base import MIMEBase
import email.encoders
```

- [MIMEBase](https://docs.python.org/3/library/email.mime.html?highlight=email%20mime#email.mime.base.MIMEBase) *This is the base class for all the MIME-specific subclasses.*
- [MIMEMultipart](https://docs.python.org/3/library/email.mime.html?highlight=email%20mime#email.mime.multipart.MIMEMultipart) To send messages divided in multiple messages. Usually used for attachments.
- [MIMEText](https://docs.python.org/3/library/email.mime.html?highlight=email%20mime#email.mime.text.MIMEText) To work with plain files files.

Now, We can create our function to send emails attaching a text file.

```python
# main.py

# Methods section ....
def send_from_txt():
    """
    Send an email reading the content message from a txt file
    """
    message = MIMEMultipart('alternative')
    message['Subject'] = 'TXT file content'
    message['From'] = SENDER
    body = "Email message body"
    content = MIMEText(body, 'plain')
    message.attach(content)

    # process text file
    file_open = open('text.txt', 'rb')
    file_content = MIMEText(_text=file_open.read(), _charset='utf-8')
    file_open.close()
    message.attach(file_content)

    # Send email
    send_email(message.as_string())
```

Add the option to our main function.

```python
# main.py

# Main section ...
if __name__ == "__main__":
	# ....
    
    if len(sys.argv)> 1 :
        # if ....
        elif sys.argv[1] == '--txt':
            print("Sending an email from a text file content")
            send_from_txt()
    else:
        # ....
```

Create a `.txt` file with a message.

```bash
# Creating file
touch text.txt

# Edit the file
nano text.txt

# Put the following message
Message from a TXT file
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --txt

# Sending an email from a text file content
# Successfully sent email
```

## Attaching files

Finally, we will send an email with a PDF attachment.

```python
# main.py

# Methods section ....
def send_attachment():
    """
    Send an email with an attachment
    """
    message = MIMEMultipart('alternative')
    message['Subject'] = 'Attachment'
    message['From'] = SENDER

    attachment = MIMEBase('application', 'octet-stream')
    attachment.set_payload(open('file.pdf', 'rb').read())
    attachment.add_header('Content-Disposition', 'attachment; filename="file.pdf"')

    email.encoders.encode_base64(attachment)
    message.attach(attachment)

    # Send email
    send_email(message.as_string())
```

Add the option to our main function.

```python
# main.py

# Main section ...
if __name__ == "__main__":
	# ....
    
    if len(sys.argv)> 1 :
        # if ....
		elif sys.argv[1] == '--attach':
            print("Sending an email with an attachment")
            send_attachment()
    else:
        # ....
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --attach

# Sending an email with an attachment
# Successfully sent email
```

**NOTE:** To test this method we have to put in our project folder a file called `file.pdf`.

## Final Words

Thanks for reading this post and don't forget to turn off the *Less secure app access* configuration.

You can find the code of this guide [here](https://gist.github.com/brayvasq/db081200debc91e1eb886a79edb1ebfe).