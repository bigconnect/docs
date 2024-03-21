# Updating User Profile Pictures

Users have profile pictures. Administrative Users can upload images in the Edit User form, and Users can update their own account information, including profile image.

### Updating a Profile Picture <a href="#updating-a-profile-picture" id="updating-a-profile-picture"></a>

To update a User profile picture follow these steps:

1. Click _My Profile_ in the User menu or if you’re an administrator, go to _Control Panel_ → _Users and Organizations_ and find the User that needs a profile picture.
2. Click the _Change_ button under the generic profile picture.
3. Select an image from your computer, crop it (if necessary), and then click _Done_.

![Upload images for user avatars in the Edit User form.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/users/updating-user-profile-pictures/images/01.png)

The Profile image is now updated.

### Changing the Default User Profile Picture Image <a href="#changing-the-default-user-profile-picture-image" id="changing-the-default-user-profile-picture-image"></a>

If no image is explicitly uploaded for a User’s profile picture, a default User icon is assigned as the User avatar comprised of the User’s initials (First Name then Last Name) over a random color.

![The default user profile picture is an icon with the user initials over a randomly colored bubble.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/users/updating-user-profile-pictures/images/02.png)

#### Disable the Use of Initials for the Default User Profile Image <a href="#disable-the-use-of-initials-for-the-default-user-profile-image" id="disable-the-use-of-initials-for-the-default-user-profile-image"></a>

1. Navigate to _Control Panel_ → _Configuration_ → _System Settings_.
2. In the Platform section, click _Users_ → _User Images_.
3. De-select _Use Initials for Default User Portrait_.

![If you disable the default initials-based profile picture, this icon is used instead.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/users/updating-user-profile-pictures/images/03.png)

Now, instead of the default icon, the icon is a generic silhouette.

#### Changing the Order of Initials in the Default User Profile Image <a href="#changing-the-order-of-initials-in-the-default-user-profile-image" id="changing-the-order-of-initials-in-the-default-user-profile-image"></a>

The default user icon (with the user initials) can be configured to work for your locale. These settings are configured in a Language Settings module.

`lang.user.default.portrait=initials` sets the type of icon to use for avatars. The default value is _initials_. If set to initials, the next property configures which initials to display, and in what order. Alternatively, specify _image_, which gives you the same non-initials default image shown above.

`lang.user.initials.fields=first-name,last-name` determines which initials appear in the user portrait and in what order. The setting here only matters if `lang.user.default.portrait` is set to _initials_. Valid values are `first-name`, `middle-name`, `last-name`, with first and last name as the defaults.

#### Using a Custom Default Profile Image <a href="#using-a-custom-default-profile-image" id="using-a-custom-default-profile-image"></a>

You can use your own default profile image:

1. Create at least one image that is a 100x100 px square in .png or .jpg format.
2. Place it somewhere on the application server’s classpath. For example, in Tomcat you could place it in the `tomcat/webapps/ROOT/WEB-INF/classes` folder.
3.  Set the following property in a `portal-ext.properties` file:

    ```properties
    image.default.user.portrait=image-filename-here.png
    ```

    This overrides the value of this portal property:

    ```properties
    image.default.user.portrait=com/liferay/portal/dependencies/user_portrait.png
    ```

    TIP

    If you use the binary gender field to collect information on users’ genders, then you have two default images to override. Set these properties instead: :: image.default.user.female.portrait=image-filename.png image.default.user.male.portrait=image-filename.png
4. Restart the application server.
