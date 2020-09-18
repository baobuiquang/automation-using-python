
# Automation using Python

## Problem

You work for an online fruits store, and you need to develop **a system that will update the catalog information with data provided by your suppliers**. The suppliers send the data as large images with an associated description of the products in two files (`.TIF` for the image and `.txt` for the description). The images need to be converted to smaller `.jpeg` images and the text needs to be turned into an `HTML` file that shows the image and the product description. The contents of the HTML file need to be uploaded to a web service that is already running using Django. You also need to gather the name and weight of all fruits from the `.txt` files and use a Python request to upload it to your Django server.

You will create a Python script that will **process the images and descriptions** and then **update your company's online website to add the new products**.

Once the task is complete, **the supplier should be notified with an email** that indicates the total weight of fruit that were uploaded. The email should have a **`.PDF` attached** with the name of the fruit and its total weight.

Finally, in parallel to the automation running, we want to **check the health of the system and send an email if something goes wrong**.

## Resouces
- **Linux-instance VM** (Virtual Machine) through local SSH (Secure Shell) Client provided by [Qwiklabs](https://www.qwiklabs.com/)
- **SSH client** by [PuTTY](https://www.putty.org/)

## Solution - Table of Contents
- [Fetching supplier data](#Fetching-supplier-data)
- [Working with supplier images](#Working-with-supplier-images)
- [Uploading images to web server](#Uploading-images-to-web-server)
- [Uploading the descriptions](#Uploading-the-descriptions)
- [Generate a PDF report](#Generate-a-PDF-report)
- [Send report through email](#Send-report-through-email)
- [System's health check and report](#Systems-health-check-and-report)

### Source Code
You can find all my source code files (Python) at [here](https://github.com/buiquangbao/automation-using-python/tree/master/Automating%20Real-World%20Tasks%20with%20Python).

## Fetching supplier data

You'll first need to get the information from the supplier that is currently stored in a Google Drive file. The supplier has sent data as large images with an associated description of the products in two files (`.TIF` for the image and `.txt` for the description).

Here, you'll find two script files `download_drive_file.sh` and the `example_upload.py` files. You can view it by using the following command.

```
ls ~/
```

Output:
```
download_drive_file.sh  example_upload.py
```

To download the file from the supplier onto our linux-instance virtual machine we will first grant executable permission to the `download_drive_file.sh` script.

```
sudo chmod +x ~/download_drive_file.sh
```

Run the `download_drive_file.sh` shell script with the following arguments:

```
./download_drive_file.sh 1LePo57dJcgzoK4uiI_48S01Etck7w_5f supplier-data.tar.gz
```

Output
```
--2020-09-18 12:46:00--  https://docs.google.com/uc?export=download&confirm=TeMk&id=1LePo57dJcgzoK4uiI_48S01Etck7w_5f
Resolving docs.google.com (docs.google.com)... 172.217.214.102, 172.217.214.101, 172.217.214.100, ...
Connecting to docs.google.com (docs.google.com)|172.217.214.102|:443... connected.
HTTP request sent, awaiting response... 302 Moved Temporarily
Location: https://doc-14-0k-docs.googleusercontent.com/docs/securesc/7sglqv0lufrg0jg1nr1k0fak41ir6d02/b27p4njbfud4as1h4a0s1a4p98a878jj/1600433100000/14227514949285994461/00904766887789157886Z/1LePo57dJcgzoK4uiI_48S01Etck7w_5f?e=download [following]
--2020-09-18 12:46:00--  https://doc-14-0k-docs.googleusercontent.com/docs/securesc/7sglqv0lufrg0jg1nr1k0fak41ir6d02/b27p4njbfud4as1h4a0s1a4p98a878jj/1600433100000/14227514949285994461/00904766887789157886Z/1LePo57dJcgzoK4uiI_48S01Etck7w_5f?e=download
Resolving doc-14-0k-docs.googleusercontent.com (doc-14-0k-docs.googleusercontent.com)... 64.233.191.132, 2607:f8b0:4001:c0c::84
Connecting to doc-14-0k-docs.googleusercontent.com (doc-14-0k-docs.googleusercontent.com)|64.233.191.132|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://docs.google.com/nonceSigner?nonce=fkj4a4lidrnhs&continue=https://doc-14-0k-docs.googleusercontent.com/docs/securesc/7sglqv0lufrg0jg1nr1k0fak41ir6d02/b27p4njbfud4as1h4a0s1a4p98a878jj/1600433100000/14227514949285994461/00904766887789157886Z/1LePo57dJcgzoK4uiI_48S01Etck7w_5f?e%3Ddownload&hash=o3j78n6pplsmv1ohcabadojjd68fhv5j [following]
--2020-09-18 12:46:00--  https://docs.google.com/nonceSigner?nonce=fkj4a4lidrnhs&continue=https://doc-14-0k-docs.googleusercontent.com/docs/securesc/7sglqv0lufrg0jg1nr1k0fak41ir6d02/b27p4njbfud4as1h4a0s1a4p98a878jj/1600433100000/14227514949285994461/00904766887789157886Z/1LePo57dJcgzoK4uiI_48S01Etck7w_5f?e%3Ddownload&hash=o3j78n6pplsmv1ohcabadojjd68fhv5j
Connecting to docs.google.com (docs.google.com)|172.217.214.102|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://doc-14-0k-docs.googleusercontent.com/docs/securesc/7sglqv0lufrg0jg1nr1k0fak41ir6d02/b27p4njbfud4as1h4a0s1a4p98a878jj/1600433100000/14227514949285994461/00904766887789157886Z/1LePo57dJcgzoK4uiI_48S01Etck7w_5f?e=download&nonce=fkj4a4lidrnhs&user=00904766887789157886Z&hash=mf15mgbeqkb3pftoctspoimhgm2u9lko [following]
--2020-09-18 12:46:00--  https://doc-14-0k-docs.googleusercontent.com/docs/securesc/7sglqv0lufrg0jg1nr1k0fak41ir6d02/b27p4njbfud4as1h4a0s1a4p98a878jj/1600433100000/14227514949285994461/00904766887789157886Z/1LePo57dJcgzoK4uiI_48S01Etck7w_5f?e=download&nonce=fkj4a4lidrnhs&user=00904766887789157886Z&hash=mf15mgbeqkb3pftoctspoimhgm2u9lko
Connecting to doc-14-0k-docs.googleusercontent.com (doc-14-0k-docs.googleusercontent.com)|64.233.191.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/gzip]
Saving to: ‘supplier-data.tar.gz’

supplier-data.tar.gz      [    <=>                  ]  84.55M   128MB/s    in 0.7s

2020-09-18 12:46:01 (128 MB/s) - ‘supplier-data.tar.gz’ saved [88653399]
```

Check again:

```
ls
```

Output:
```
download_drive_file.sh  example_upload.py  supplier-data.tar.gz
```

You have now downloaded a file named `supplier-data.tar.gz` containing the supplier's data. Let's extract the contents from this file using the following command:

```
tar xf ~/supplier-data.tar.gz
```

This creates a directory named `supplier-data`, that contains subdirectories named `images` and `descriptions`.

```
ls
```

Output:
```
download_drive_file.sh  example_upload.py  supplier-data  supplier-data.tar.gz
```

List contents of the `supplier-data` directory using the following command:

```
ls ~/supplier-data
```

Output:
```
descriptions  images
```

The subdirectory images contain `images` of various fruits, while the `descriptions` subdirectory has text files containing the description of each fruit. You can have a look at any of these text files using `cat` command.

```
cat ~/supplier-data/descriptions/001.txt
```

Output:
```
Apple
500 lbs
Apple is one of the most nutritious and healthiest fruits. It is very rich in antioxidants and dietary fiber. Moderate consumption can not only increase satiety, but also help promote bowel movements. Apple also contains minerals such as calcium and magnesium, which can help prevent and delay bone loss and maintain bone health. It is good for young and old. 
```

The first line contains the name of the fruit followed by the weight of the fruit and finally the description of the fruit.

## Working with supplier images

In this section, you will write a Python script named `changeImage.py` to process the supplier images. You will be using the `PIL` library to update all images within `~/supplier-data/images` directory to the following specifications:

- **Size**: Change image resolution from **3000x2000** to **600x400** pixel

- **Format**: Change image format from `.TIFF` to `.JPEG`

Create and open the file using `nano` editor.

```
nano ~/changeImage.py
```

Add a shebang line in the first line.

```
#!/usr/bin/env python3
```

This is the challenge section, where you will be writing a script that satisfies the above objectives.

*Note*: The raw images from `images` subdirectory contains alpha transparency layers. So, it is better to first convert `RGBA` 4-channel format to `RGB` 3-channel format before processing the images. Use `convert("RGB")` method for converting `RGBA` to `RGB` image.

After processing the images, save them in the same path `~/supplier-data/images`, with a `JPEG` extension.

**`changeImage.py`**
```python
#!/usr/bin/env python3

import os
from PIL import Image

path = os.path.expanduser('~') + '/supplier-data/images/'
for image in os.listdir(path):
	if '.tiff' in image and '.' not in image[0]:
		img = Image.open(path + image)
		img.resize((600, 400)).convert("RGB").save(path + image.split('.')[0] + '.jpeg' , 'jpeg')
		img.close()
```

Once you have completed editing the `changeImage.py` script, save the file by clicking Ctrl-o, Enter key, and Ctrl-x.

Grant executable permissions to the `changeImage.py` script.

```
sudo chmod +x ~/changeImage.py
```

Now run the `changeImage.py` script:

```
./changeImage.py
```

Now, let's check the specifications of the images you just updated. Open any image using the following command:

```
file ~/supplier-data/images/003.jpeg
```

Output:
```
/home/student-02-bc4a98210200/supplier-data/images/001.jpeg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, baseline, precision 8, 600x400, frames 3
```

## Uploading images to web server

You have modified the fruit images through `changeImage.py` script. Now, you will have to upload these modified images to the web server that is handling the fruit catalog. To do that, you'll have to use the Python **`requests` module** to send the file contents to the `[linux-instance-IP-Address]/upload` URL.

Copy the `external IP address` of your instance from the Connection Details Panel on the left side and enter the IP address in a new web browser tab. This opens a web page displaying the text "Fruit Catalog".

In the home directory, you'll have a script named `example_upload.py` to upload images to the running fruit catalog web server. To view the `example_upload.py` script use the `cat` command.

```
cat ~/example_upload.py
```

Output:
```
#!/usr/bin/env python3
import requests

# This example shows how a file can be uploaded using
# The Python Requests module

url = "http://localhost/upload/"
with open('/usr/share/apache2/icons/icon.sheet.png', 'rb') as opened:
    r = requests.post(url, files={'file': opened})
```

In this script, we are going to upload a sample image named `icon.sheet.png`.

Grant executable permission to the `example_upload.py` script.

```
sudo chmod +x ~/example_upload.py
```

Execute the `example_upload.py` script, which will upload the images.

```
./example_upload.py
```

Now check out that the file `icon.sheet.png` was uploaded to the web server by visiting the URL `[linux-instance-IP-Address]/media/images/`, followed by clicking on the file name.

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img02.png)

In a similar way, you are going to write a script named `supplier_image_upload.py` that takes the jpeg images from the `supplier-data/images` directory that you've processed previously and uploads them to the web server fruit catalog.

Use the `nano` editor to create a file named `supplier_image_upload.py`:

```
nano ~/supplier_image_upload.py
```

Complete the script with the same technique as used in the file `example_upload.py`.

**`supplier_image_upload.py`**
```python
#!/usr/bin/env python3

import requests
import os

# This example shows how a file can be uploaded using
# The Python Requests module
url = "http://localhost/upload/"
IMAGE_DIR = os.path.expanduser('~') + '/supplier-data/images/'
list_image = os.listdir(IMAGE_DIR)
jpeg_images = [image_name for image_name in list_image if '.jpeg' in image_name]

for image in jpeg_images:
  with open(IMAGE_DIR + image, 'rb') as opened:
    r = requests.post(url, files={'file': opened})
```

Once you have completed editing the `supplier_image_upload.py` script, save the file by typing Ctrl-o, Enter key, and Ctrl-x.

Grant executable permission to the `changeImage.py` script.

```
sudo chmod +x ~/supplier_image_upload.py
```

Run the `changeImage.py` script.

```
./supplier_image_upload.py
```

Refresh the URL opened earlier, and now you should find all the images uploaded successfully.

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img03.png)

## Uploading the descriptions

The Django server **is already set up** to show the fruit catalog for your company. You can visit the main website by entering `linux-instance-IP-Address` in the URL bar or by removing `/media/images` from the existing URL opened earlier.

Check out the Django REST framework, by navigating to `linux-instance-IP-Address/fruits` in your browser.

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img04.png)

Currently, there are no products in the fruit catalog web-server. You can create a test fruit entry by entering the following into the content field:

`
{"name": "Test Fruit", "weight": 100, "description": "This is the description of my test fruit", "image_name": "icon.sheet.png"}
`

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img05.png)

After entering the above data into the content field click on the POST button. Now visit the main page of your website (by going to `http://[linux-instance-external-IP]`), and the new test fruit you uploaded appears.

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img06.png)

To add fruit images and their descriptions from the supplier on the fruit catalog web-server, create a new Python script that will automatically POST the fruit images and their respective description in `JSON` format.

Write a Python script named `run.py` to process the text files (`001.txt`, `003.txt` ...) from the `supplier-data/descriptions` directory. The script should turn the data into a `JSON` dictionary by adding all the required fields, including the image associated with the fruit (`image_name`), and uploading it to `http://[linux-instance-external-IP]/fruits` using the Python `requests` library.

Create `run.py` using the nano editor:

```
nano ~/run.py
```

Add the shebang line and import necessary libraries.

```
#! /usr/bin/env python3
```

```
import os
import requests
```

Now, you'll have to process the `.txt` files (named `001.txt`, `002.txt`, ...) in the `supplier-data/descriptions/` directory and save them in a data structure so that you can then upload them via `JSON`. Note that all files are written in the following format, with each piece of information on its own line:

- name

- weight

- description

The data model in the Django application `fruit` has the following fields: `name`, `weight`, `description` and `image_name`. The `weight` field is defined as an integer field. So when you process the `weight` information of the fruit from the `.txt` file, you need to convert it into an integer. For example if the `weight` is "500 lbs", you need to drop "lbs" and convert "500" to an integer.

The `image_name` field will allow the system to find the image associated with the fruit. Don't forget to add all fields, including the `image_name`! The final `JSON` object should be similar to:

`
{"name": "Watermelon", "weight": 500, "description": "Watermelon is good for relieving heat, eliminating annoyance and quenching thirst. It contains a lot of water, which is good for relieving the symptoms of acute fever immediately. The sugar and salt contained in watermelon can diuretic and eliminate kidney inflammation. Watermelon also contains substances that can lower blood pressure.", "image_name": "010.jpeg"}
`

Iterate over all the fruits and use post method from Python `requests` library to upload all the data to the URL `http://[linux-instance-external-IP]/fruits`

**`run.py`**
```python
#!/usr/bin/env python3

import os 
import requests

BASEPATH_SUPPLIER_TEXT_DES = os.path.expanduser('~') + '/supplier-data/descriptions/'
list_text_files = os.listdir(BASEPATH_SUPPLIER_TEXT_DES)

BASEPATH_SUPPLIER_IMAGE = os.path.expanduser('~') + '/supplier-data/images/'
list_image_files = os.listdir(BASEPATH_SUPPLIER_IMAGE)
list_images = [image_name for image_name in list_image_files if '.jpeg' in image_name]

list = []
for text_file in list_text_files:
	with open(BASEPATH_SUPPLIER_TEXT_DES + text_file, 'r') as f:
		data = {"name":f.readline().rstrip("\n"),
                "weight":int(f.readline().rstrip("\n").split(' ')[0]),
                "description":f.readline().rstrip("\n")}
		for image_file in list_images:
			if image_file.split('.')[0] in text_file.split('.')[0]:
				data['image_name'] = image_file

		list.append(data)

for item in list:
    resp = requests.post('http://127.0.0.1:80/fruits/', json=item)
    if resp.status_code != 201:
        raise Exception('POST error status={}'.format(resp.status_code))
    print('Created feedback ID: {}'.format(resp.json()["id"]))
```

Once you complete editing `run.py` script, save the file by clicking Ctrl-o, Enter key, and Ctrl-x.

Grant executable permission to the `run.py` script.

```
sudo chmod +x ~/run.py
```

Run the `run.py` script:

```
./run.py
```

Output:
```
Created feedback ID: 2
Created feedback ID: 3
Created feedback ID: 4
Created feedback ID: 5
Created feedback ID: 6
Created feedback ID: 7
Created feedback ID: 8
Created feedback ID: 9
Created feedback ID: 10
Created feedback ID: 11
```

Now go to the main page of your website (by going to `http://[linux-instance-IP-Address]/`) and check out how the new fruits appear.

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img07.png)

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img08.png)

## Generate a PDF report

Once the `images` and `descriptions` have been uploaded to the fruit store web-server, you will have to generate a `PDF` file to send to the supplier, indicating that the data was correctly processed. To generate `PDF` reports, you can use the `ReportLab` library. The content of the report should look like this:

```
Processed Update on <Today's date>

[blank line]

name: Apple

weight: 500 lbs

[blank line]

name: Avocado

weight: 200 lbs

[blank line]

...
```

Create a script `reports.py` to generate `PDF` report to supplier using the `nano` editor:

```
nano ~/reports.py
```

Add a shebang line in the first line.

```
#!/usr/bin/env python3
```

Using the `reportlab` Python library, define the method `generate_report` to build the `PDF` reports. We have already covered how to generate `PDF` reports in an earlier lesson; you will want to use similar concepts to create a `PDF` report named `processed.pdf`.

**`reports.py`**
```python
#!/usr/bin/env python3

from reportlab.platypus import SimpleDocTemplate
from reportlab.platypus import Paragraph, Spacer, Image
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.lib import colors

def generate_report(filename, title, additional_info):
	styles = getSampleStyleSheet()	report = SimpleDocTemplate(filename)
	report_title = Paragraph(title, styles["h1"])
	report_info = Paragraph(additional_info, styles["Normal"])
	empty_line = Spacer(1,20)
	report.build([report_title, empty_line, report_info])

```

Once you have finished editing the script `reports.py`, save the file by typing Ctrl-o, Enter key, and Ctrl-x.

Create another script named `report_email.py` to process supplier fruit description data from `supplier-data/descriptions` directory. Use the following command to create `report_email.py`.

```
nano ~/report_email.py
```

Add a shebang line.

```
#!/usr/bin/env python3
```

Import all the necessary libraries(`os`, `datetime` and `reports`) that will be used to process the text data from the `supplier-data/descriptions` directory into the format below:

```
name: Apple

weight: 500 lbs

[blank line]

name: Avocado

weight: 200 lbs

[blank line]

...
```

Once you have completed this, call the main method which will process the data and call the `generate_report` method from the `reports` module:

```
if __name__ == "__main__":
```

You will need to pass the following arguments to the `reports.generate_report` method: the text description processed from the text files as the `paragraph` argument, the report title as the `title` argument, and the file path of the `PDF` to be generated as the `attachment` argument (use '`/tmp/processed.pdf`')

```
reports.generate_report(attachment, title, paragraph)
```

**`report_email.py`**
```python
#!/usr/bin/env python3

import reports
import emails
import os 
from datetime import date

BASEPATH_SUPPLIER_TEXT_DES = os.path.expanduser('~') + '/supplier-data/descriptions/'
list_text_files = os.listdir(BASEPATH_SUPPLIER_TEXT_DES)

report = []

def process_data(data):
	for item in data:
		report.append("name: {}<br/>weight: {}\n".format(item[0], item[1]))
	return report

text_data = []
for text_file in list_text_files:
	with open(BASEPATH_SUPPLIER_TEXT_DES + text_file, 'r') as f:
		text_data.append([line.strip() for line in f.readlines()])
		f.close()

if __name__ == "__main__":

	summary = process_data(text_data)

	# Generate a paragraph that contains the necessary summary
	paragraph = "<br/><br/>".join(summary)

	# Generate the PDF report
	title = "Processed Update on {}\n".format(date.today().strftime("%B %d, %Y"))
	attachment = "/tmp/processed.pdf"

	reports.generate_report(attachment, title, paragraph)

	# Send the email
	# subject = "Upload Completed - Online Fruit Store"
	# sender = "automation@example.com"
	# receiver = "{}@example.com".format(os.environ.get('USER'))
	# body = "All fruits are uploaded to our website successfully. A detailed list is attached to this email."
	# message = emails.generate_email(sender, receiver, subject, body, attachment)
	# emails.send_email(message)
```

Once you have completed the `report_email.py` script. Save the file by typing Ctrl-o, Enter key, and Ctrl-x.

## Send report through email

Once the PDF is generated, you need to send the email using the `emails.generate_email()` and `emails.send_email()` methods.

Create `emails.py` using the `nano` editor using the following command:

```
nano ~/emails.py
```

Define `generate_email` and `send_email` methods by importing necessary libraries.

**`emails.py`**
```python
#!/usr/bin/env python3

import email.message
import mimetypes
import os.path
import smtplib

def generate_email(sender, recipient, subject, body, attachment_path):
	"""Creates an email with an attachement."""
	# Basic Email formatting
	message = email.message.EmailMessage()
	message["From"] = sender
	message["To"] = recipient
	message["Subject"] = subject
	message.set_content(body)

	# Process the attachment and add it to the email
	attachment_filename = os.path.basename(attachment_path)
	mime_type, _ = mimetypes.guess_type(attachment_path)
	mime_type, mime_subtype = mime_type.split('/', 1)

	with open(attachment_path, 'rb') as ap:
		message.add_attachment(ap.read(), maintype=mime_type, subtype=mime_subtype, filename=attachment_filename)

	return message

def send_email(message):
	"""Sends the message to the configured SMTP server."""
	mail_server = smtplib.SMTP('localhost')
	mail_server.send_message(message)
	mail_server.quit()

def generate_error_report(sender, recipient, subject, body):
	message = email.message.EmailMessage()
	message["From"] = sender
	message["To"] = recipient
	message["Subject"] = subject
	message.set_content(body)

	return message
```

Once you have finished editing the `emails.py` script, save the file by typing Ctrl-o, Enter key, and Ctrl-x.

Now, open the `report_email.py` script using the nano editor:

```
nano ~/report_email.py
```

Once you define the `generate_email` and `send_email` methods, call the methods under the main method after creating the `PDF` report:

```
if __name__ == "__main__":
```

Use the following details to pass the parameters to `emails.generate_email()`:

- **From**: `automation@example.com`

- **To**: `username@example.com` (Replace `username` with the `username` given in the Connection Details Panel on the right hand side.)

- **Subject line**: Upload Completed - Online Fruit Store

- **E-mail Body**: All fruits are uploaded to our website successfully. A detailed list is attached to this email.

- **Attachment**: Attach the path to the file `processed.pdf`

Once you have finished editing the `report_email.py` script, save the file by typing Ctrl-o, Enter key, and Ctrl-x.

**`report_email.py`**
```python
#!/usr/bin/env python3

import reports
import emails
import os 
from datetime import date

BASEPATH_SUPPLIER_TEXT_DES = os.path.expanduser('~') + '/supplier-data/descriptions/'
list_text_files = os.listdir(BASEPATH_SUPPLIER_TEXT_DES)

report = []

def process_data(data):
	for item in data:
		report.append("name: {}<br/>weight: {}\n".format(item[0], item[1]))
	return report

text_data = []
for text_file in list_text_files:
	with open(BASEPATH_SUPPLIER_TEXT_DES + text_file, 'r') as f:
		text_data.append([line.strip() for line in f.readlines()])
		f.close()

if __name__ == "__main__":

	summary = process_data(text_data)

	# Generate a paragraph that contains the necessary summary
	paragraph = "<br/><br/>".join(summary)

	# Generate the PDF report
	title = "Processed Update on {}\n".format(date.today().strftime("%B %d, %Y"))
	attachment = "/tmp/processed.pdf"

	reports.generate_report(attachment, title, paragraph)

	# Send the email
	subject = "Upload Completed - Online Fruit Store"
	sender = "automation@example.com"
	receiver = "{}@example.com".format(os.environ.get('USER'))
	body = "All fruits are uploaded to our website successfully. A detailed list is attached to this email."
	message = emails.generate_email(sender, receiver, subject, body, attachment)
	emails.send_email(message)
```

Grant executable permissions to the script `report_email.py`.

```
sudo chmod +x ~/report_email.py
```

Run the `report_email.py` script.

```
./report_email.py
```

Now, check the webmail by visiting `[linux-instance-external-IP]/webmail`. Here, you'll need a login to roundcube using the username and password mentioned in the Connection Details Panel on the left hand side, followed by clicking Login.

Now you should be able to see your inbox, with one unread email. Open the mail by double clicking on it. There should be a report in `PDF` format attached to the mail. View the report by opening it.

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img09.png)

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img10.png)

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img11.png)

## System's health check and report

This is the last part of the lab, where you will have to write a Python script named `health_check.py` that will run in the background monitoring some of your system statistics: CPU usage, disk space, available memory and name resolution. Moreover, this Python script should send an email if there are problems, such as:

- Report an error if *CPU* usage is over 80%

- Report an error if available *disk space* is lower than 20%

- Report an error if available *memory* is less than 500MB

- Report an error if the *hostname "localhost"* cannot be resolved to "127.0.0.1"

Create a python script named `health_check.py` using the nano editor:

```
nano ~/health_check.py
```

Add a shebang line.

```
#!/usr/bin/env python3
```

Import the necessary Python libraries (eg. `shutil`, `psutil`) to write this script.

Complete the script to check the system statistics every 60 seconds, and in event of any issues detected among the ones mentioned above, an email should be sent with the following content:

- **From**: `automation@example.com`

- **To**: `username@example.com` (Replace username with the username given in the Connection Details Panel on the right hand side.)

- **Subject line**:

  - CPU usage is over 80% -> Error - CPU usage is over 80%

  - Available disk space is lower than 20% -> Error - Available disk space is less than 20%

  - Available memory is less than 500MB -> Error - Available memory is less than 500MB

  - Hostname "localhost" cannot be resolved to "127.0.0.1" -> Error - localhost cannot be resolved to 127.0.0.1

- **E-mail Body**: Please check your system and resolve the issue as soon as possible.

*Note*: You must be careful while defining the `generate_email()` method in the `emails.py` script or you can create a separate `generate_error_report()` method for handling non-attachment email.

**`health_check.py`**
```python
#!/usr/bin/env python3

import os
import shutil
import psutil
import socket
from emails import generate_error_report, send_email

def check_cpu_usage():
	"""Verifies that there's enough unused CPU"""
	usage = psutil.cpu_percent(1)
	return usage > 80

def check_disk_usage(disk):
	"""Verifies that there's enough free space on disk"""
	du = shutil.disk_usage(disk)
	free = du.free / du.total * 100
	return free > 20

def check_available_memory():
	"""available memory in linux-instance, in byte"""
	available_memory = psutil.virtual_memory().available/(1024*1024)
	return available_memory > 500

def check_localhost():
	"""check localhost is correctly configured on 127.0.0.1"""
	localhost = socket.gethostbyname('localhost')
	return localhost == '127.0.0.1'

if check_cpu_usage():
	error_message = "CPU usage is over 80%"
elif not check_disk_usage('/'):
	error_message = "Available disk space is less than 20%"
elif not check_available_memory():
	error_message = "Available memory is less than 500MB"
elif not check_localhost():
	error_message = "localhost cannot be resolved to 127.0.0.1"
else:
	pass

# send email if any error reported
if __name__ == "__main__":
	try:
		sender = "automation@example.com"
		receiver = "{}@example.com".format(os.environ.get('USER'))
		subject = "Error - {}".format(error_message)
		body = "Please check your system and resolve the issue as soon as possible"
		message = generate_error_report(sender, receiver, subject, body)
		send_email(message)
	except NameError:
		pass

```

Once you have completed the `health_check.py` script. Save the file by typing Ctrl-o, Enter key, and Ctrl-x.

Grant executable permissions to the script `health_check.py`.

```
sudo chmod +x ~/health_check.py
```

Run `health_check.py`

```
./health_check.py
```

Next, go to the webmail inbox and refresh it. There should only be an email something goes wrong, so hopefully you don't see a new email.

To test out your script, you can install the `stress` tool.

```
sudo apt install stress
```

Output:
```
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  stress
0 upgraded, 1 newly installed, 0 to remove and 2 not upgraded.
Need to get 21.3 kB of archives.
After this operation, 50.2 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian stretch/main amd64 stress amd64 1.0.4-2 [21.3 kB]
Fetched 21.3 kB in 0s (415 kB/s)
Selecting previously unselected package stress.
(Reading database ... 56165 files and directories currently installed.)
Preparing to unpack .../stress_1.0.4-2_amd64.deb ...
Unpacking stress (1.0.4-2) ...
Processing triggers for man-db (2.7.6.1-2) ...
Setting up stress (1.0.4-2) ...
```

Next, call the tool using a good number of CPUs to fully load our CPU resources:

```
stress --cpu 8
```

Allow the stress test to run, as it will maximize our CPU utilization. Now run `health_check.py` by opening **another SSH connection** to the `linux-instance`. Navigate to `Accessing the virtual machine` on the navigation pane on the right-hand side to open another connection to the instance.

Now run the script:

```
./health_check.py
```

Check your inbox for any new email.

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img12.png)

![Image](https://github.com/buiquangbao/automation-using-python/blob/master/assets/img13.png)

Close the `stress --cpu` command by clicking Ctrl-c.






