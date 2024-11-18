# Image

The image component is used to display images. To add an image, link to an image using the URL setting. For example, add `<https://source.unsplash.com/random`> to the URL setting, and a random image from Unsplash will be added to your app.

<figure><img src="https://files.readme.io/ee9f74c23386dd606c42d40ca0932ee8ebade550a5dd345befdcf2e8db0d4e7a-Screenshot_2024-09-27_at_13.34.20.png" alt=""><figcaption></figcaption></figure>

### Displaying images from an attachment

Images can be uploaded as Attachments and you may wish to display these outside of a Table.

**Data**

Create a table called _Vehicles_ with a category name and an attachment column for the images. Upload your images.

<figure><img src="https://files.readme.io/ee306458656552391ad9cc5b3b58b4b3eb62f5cd7a773c8544d4f6fb1bc16d9f-Screenshot_2024-09-27_at_13.38.54.png" alt=""><figcaption></figcaption></figure>

**Design**

1. Add a Data Provider for the _Vehicles_ table.
2. Add a Repeater to the data provider. This will add a container for each _Vehicles_ row.
3. Add a Headline to display a title for each _Vehicles_ category.\
   The Binding in this case is `{{ Vehicles Repeater.Vehicles.Category }}`
4. Underneath the headline, add a data provider for the "Images". Attachments List column types which we created are saved in JSON Arrays.
5.  Change the source of the data provider to JSON / CSV

    ![](https://files.readme.io/ebc8233e5ed68317eacea5a70c31910c45228defe19e3841702f78f41316d604-Screenshot\_2024-09-27\_at\_13.54.30.png)
6. Click the settings cog beside JSON / CSV and paste this binding inside it `{{ New Repeater.Vehicles.Images }}`
7. Add a repeater so that each attachment image can be displayed.
8. Finally, add an Image component within the repeater. Paste the below binding inside the URL setting of the image component. `return $("New Repeater.Vehicles.Images")[$("images repeater.Row index")].url`

**Result**

<figure><img src="https://files.readme.io/34b25365e1aca82fdf1736119cf24e554ff2303e5bf7e3a17a6377dab5ec7580-Screenshot_2024-09-27_at_13.53.59.png" alt=""><figcaption></figcaption></figure>

**Single or fixed number of image attachments**

If you know there will only be a single image attachment, you should make sure to use the [Attachment](https://docs.budibase.com/docs/attachments#attachment) column type. Then you do not need to add a data provider and repeater for the attachment column. You will have access to the image bindings.

For a fixed number of attachments, simply adjust the index in the URL for each image component. This was shown in the above example

### Displaying images from an S3 bucket

You may wish to store your image files in an external S3 bucket instead of the built-in MinIO bucket.

**Data**

Upload your images to your S3 bucket. You will need to allow public access to the images in AWS:

<figure><img src="https://files.readme.io/9bdcee7-Screenshot_2022-11-21_at_17.08.00.png" alt=""><figcaption></figcaption></figure>

Next add your S3 Datasource with a simple **Read** query to list all of the files in the bucket.

<figure><img src="https://files.readme.io/53c70303303da5e7e808113f2066ca36df3018dfb8cabdf1db8b4821ebc92059-Screenshot_2024-09-27_at_13.58.41.png" alt=""><figcaption></figcaption></figure>

**Design**

Add a Repeater Block with the _Read_ query as its datasource. Nest your image components, using the following URL binding: `https://<bucket-name>.s3.us-west-1.amazonaws.com/{{ New Repeater block.Read.Key }}`

<figure><img src="https://files.readme.io/ad0e80803efd095809b8a639487fce8557a3e603bba611102901a5a69651b8a6-Screenshot_2024-09-27_at_14.01.00.png" alt=""><figcaption></figcaption></figure>

### Image carousel

There is no built-in slideshow component, however there is a tutorial here for building one.

### Background image

Add the background image component. This component can take children, which will appear in front of the background.

<figure><img src="https://files.readme.io/8db6eb1-Screenshot_2023-03-27_at_09.32.01.png" alt="Adding the background image component"><figcaption><p>Adding the background image component</p></figcaption></figure>

Provide a valid URL as the image source. The _Position_ property can also be edited to offset the background image.

<figure><img src="https://files.readme.io/9891d335a3b139505e587e980a97617d536e4e8f018ba1e533b1f09a88aeac43-Screenshot_2024-09-27_at_14.04.37.png" alt=""><figcaption></figcaption></figure>
