# Automation using Python

### Problem:

You work for an online fruits store, and you need to develop a system that will update the catalog information with data provided by your suppliers. The suppliers send the data as large images with an associated description of the products in two files (.TIF for the image and .txt for the description). The images need to be converted to smaller jpeg images and the text needs to be turned into an HTML file that shows the image and the product description. The contents of the HTML file need to be uploaded to a web service that is already running using Django. You also need to gather the name and weight of all fruits from the .txt files and use a Python request to upload it to your Django server.

You will create a Python script that will process the images and descriptions and then update your company's online website to add the new products.

Once the task is complete, the supplier should be notified with an email that indicates the total weight of fruit (in lbs) that were uploaded. The email should have a PDF attached with the name of the fruit and its total weight (in lbs).

Finally, in parallel to the automation running, we want to check the health of the system and send an email if something goes wrong.

### What to do:

Write a script that summarizes and processes sales data into different categories

Generate a PDF using Python

Automatically send a PDF by email

Write a script to check the health status of the system

### Fetching supplier data

You'll first need to get the information from the supplier that is currently stored in a Google Drive file. The supplier has sent data as large images with an associated description of the products in two files (.TIF for the image and .txt for the description).

Here, you'll find two script files `download_drive_file.sh` and the `example_upload.py` files. You can view it by using the following command.

```
ls ~/
```

To download the file from the supplier onto our linux-instance virtual machine we will first grant executable permission to the `download_drive_file.sh` script.

```
sudo chmod +x ~/download_drive_file.sh
```

Run the download_drive_file.sh shell script with the following arguments:

```
./download_drive_file.sh 1LePo57dJcgzoK4uiI_48S01Etck7w_5f supplier-data.tar.gz
```

You have now downloaded a file named `supplier-data.tar.gz` containing the supplier's data. Let's extract the contents from this file using the following command:

```
tar xf ~/supplier-data.tar.gz
```

This creates a directory named `supplier-data`, that contains subdirectories named `images` and `descriptions`.

List contents of the `supplier-data` directory using the following command:

```
ls ~/supplier-data
```

The subdirectory images contain `images` of various fruits, while the `descriptions` subdirectory has text files containing the description of each fruit. You can have a look at any of these text files using `cat` command.

```
cat ~/supplier-data/descriptions/007.txt
```

The first line contains the name of the fruit followed by the weight of the fruit and finally the description of the fruit.

### Working with supplier images

### Uploading images to web server

### Uploading the descriptions

### Generate a PDF report and send it through email

### Health check









